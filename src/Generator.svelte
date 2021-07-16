<script>
    import { onMount } from "svelte";

    export let input;
    export let fontsize = 8;
    export let linespacing = 1.1;
    export let jahyphenation = true;

    let c;
    let dummy;

    let canvas_width = 600;
    let canvas_height = 600;

    let margin = 30;

    let actual_height = canvas_width - margin;
    let actual_width = canvas_width - margin;

    let text_size;

    var image;

    function getWidth(target_letters, font_family, font_size) {
        let dummy_ctx = dummy.getContext("2d");
        dummy_ctx.font = `${font_size}px ${font_family}`;
        const metrics = dummy_ctx.measureText(target_letters);

        return metrics.width;
    }

    function getHeight(target_letters, font_family, font_size) {
        let dummy_ctx = dummy.getContext("2d");
        dummy_ctx.font = `${font_size}px ${font_family}`;
        const metrics = dummy_ctx.measureText(target_letters);

        return (
            metrics.actualBoundingBoxAscent + metrics.actualBoundingBoxDescent
        );
    }

    function lineWrap(input_paragraphs) {
        let line_wrapped = [];
        input_paragraphs.forEach((paragraph, order) => {
            let kari = paragraph;

            while (actual_width <= getWidth(kari)) {
                let letter_count = 1;

                while (actual_width > getWidth(kari.slice(0, letter_count))) {
                    letter_count += 1;
                }
                if (
                    jahyphenation === true &&
                    "、。，．,.".indexOf(
                        kari.slice(letter_count - 1, letter_count)
                    ) !== -1
                ) {
                } else {
                    letter_count -= 1;
                }

                line_wrapped.push(kari.slice(0, letter_count));
                kari = kari.slice(letter_count);
            }
            line_wrapped.push(kari);
        });
        return line_wrapped;
    }

    function roundedRect(_ctx, x, y, width, height, radius) {
        //This function is from MDN. It is released under the CC0.
        //https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Drawing_shapes
        _ctx.beginPath();
        _ctx.moveTo(x, y + radius);
        _ctx.lineTo(x, y + height - radius);
        _ctx.arcTo(x, y + height, x + radius, y + height, radius);
        _ctx.lineTo(x + width - radius, y + height);
        _ctx.arcTo(
            x + width,
            y + height,
            x + width,
            y + height - radius,
            radius
        );
        _ctx.lineTo(x + width, y + radius);
        _ctx.arcTo(x + width, y, x + width - radius, y, radius);
        _ctx.lineTo(x + radius, y);
        _ctx.arcTo(x, y, x, y + radius, radius);
        _ctx.fill();
    }

    function write(_ctx) {
        _ctx.fillStyle = "rgb(145, 214, 227)";
        _ctx.fillRect(0, 0, canvas_width, canvas_height);
        _ctx.fillStyle = "rgb(239, 252, 255)";
        roundedRect(
            _ctx,
            margin / 2,
            margin / 2,
            actual_height,
            actual_width,
            10
        );

        let line_wrapped = lineWrap(input);

        _ctx.fillStyle = "rgb(0, 0, 0)";
        line_wrapped.forEach((paragraph, order) => {
            _ctx.fillText(
                paragraph,
                margin / 2,
                order * text_size * linespacing + margin / 2
            );
        });

        const old_img = image;

        c.toBlob((blob) => {
            image = URL.createObjectURL(blob);
        });

        URL.revokeObjectURL(old_img);
    }

    onMount(() => {
        let _ctx = c.getContext("2d");

        _ctx.textBaseline = "top";
        _ctx.font = `${(fontsize * 96) / 72}px sans-serif`;
        text_size = getHeight("あ", "sans-serif", (fontsize * 96) / 72);
    });

    $: if (c) {
        input = input;
        linespacing = linespacing;
        jahyphenation = jahyphenation;

        let _ctx = c.getContext("2d");

        _ctx.font = `${(fontsize * 96) / 72}px sans-serif`;
        text_size = getHeight("あ", "sans-serif", (fontsize * 96) / 72);

        write(_ctx);
    }
</script>

<img src={image} alt={input.join("")} />
<canvas bind:this={c} height={canvas_height} width={canvas_width} />

<canvas bind:this={dummy} height={0} width={0} />

<style>
    canvas {
        display: none;
    }
</style>
