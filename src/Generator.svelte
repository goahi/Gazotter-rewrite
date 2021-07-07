<script>
    import { onMount } from "svelte";

    export let input;
    export let fontsize = 8;

    let c;
    let dummy;

    let canvas_width = 600;
    let canvas_height = 600;

    let text_size;

    let dataURL;

    function measure(target_letters, font_family, font_size) {
        let dummy_ctx = dummy.getContext("2d");
        dummy_ctx.font = `${font_size}px ${font_family}`;
        const metrics = dummy_ctx.measureText(target_letters);

        return {
            height:
                metrics.actualBoundingBoxAscent +
                metrics.actualBoundingBoxDescent,
            width: metrics.width,
        };
    }

    function write(_ctx) {
        _ctx.clearRect(0, 0, canvas_width, canvas_height);

        input.forEach((paragraph, order) => {
            _ctx.fillText(paragraph, 10, order * text_size.height);
        });
        dataURL = c.toDataURL();
    }

    onMount(() => {
        let ctx = c.getContext("2d");

        ctx.textBaseline = "top";
        ctx.font = `${fontsize}px sans-serif`;
        text_size = measure("あ", "sans-serif", fontsize);
    });

    $: if (c) {
        input = input;
        let ctx = c.getContext("2d");

        ctx.font = `${fontsize}px sans-serif`;
        text_size = measure("あ", "sans-serif", fontsize);

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
