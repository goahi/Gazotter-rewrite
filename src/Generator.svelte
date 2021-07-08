<script>
    import { onMount } from "svelte";

    export let input;
    export let fontsize = 8;
    export let linespacing = 1.1;

    let c;
    let dummy;

    let canvas_width = 600;
    let canvas_height = 600;

    let margin = 30;

    let actual_height = canvas_width - margin / 2;
    let actual_width = canvas_width - margin / 2;

    let text_size;

    let dataURL;

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
                letter_count -= 1;

                line_wrapped.push(kari.slice(0, letter_count));
                kari = kari.slice(letter_count);
            }
            line_wrapped.push(kari);
        });
        return line_wrapped;
    }

    function write(_ctx) {
        _ctx.clearRect(0, 0, canvas_width, canvas_height);

        let line_wrapped = lineWrap(input);

        line_wrapped.forEach((paragraph, order) => {
            _ctx.fillText(
                paragraph,
                margin / 2,
                order * text_size * linespacing + margin / 2
            );
        });
        dataURL = c.toDataURL();
    }

    onMount(() => {
        let ctx = c.getContext("2d");

        ctx.textBaseline = "top";
        ctx.font = `${fontsize}px sans-serif`;
        text_size = getHeight("あ", "sans-serif", fontsize);
    });

    $: if (c) {
        input = input;
        linespacing = linespacing;
        let ctx = c.getContext("2d");

        ctx.font = `${fontsize}px sans-serif`;
        text_size = getHeight("あ", "sans-serif", fontsize);

        write(ctx);
    }
</script>

<img src={dataURL} alt={input.join("")} />
<canvas bind:this={c} height={canvas_height} width={canvas_width} />

<canvas bind:this={dummy} height={0} width={0} />

<style>
    canvas {
        display: none;
    }
</style>
