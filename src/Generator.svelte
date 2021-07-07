<script>
    export let input;
    export let fontsize = 8;

    let c;
    let ctx;

    let canvas_width = 600;
    let canvas_height = 600;

    let dataURL;

    $: if (c) {
        ctx = c.getContext("2d");

        ctx.clearRect(0, 0, canvas_width, canvas_height);
        ctx.textBaseline = "top";
        ctx.font = `${fontsize}px sans-serif`;

        input.forEach((paragraph, order) => {
            ctx.fillText(paragraph, 10, (order + 1) * 50);
        });
        dataURL = c.toDataURL();
    }
</script>

<img src={dataURL} alt={input.join("")} />
<canvas bind:this={c} height={canvas_height} width={canvas_width} />

<style>
    canvas {
        display: none;
    }
</style>
