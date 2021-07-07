<script>
    import { onMount } from "svelte";

    export let input;
    export let fontsize = 8;

    let c;
    let ctx;

    let canvas_width = 600;
    let canvas_height = 600;

    let dataURL;

    let target;

    function measure(target_letters, font_family, font_size) {
        target.style.fontFamily = font_family;
        target.style.fontSize = `${font_size}px`;

        target.textContent = target_letters;

        return { height: target.offsetHeight, width: target.offsetWidth };
    }

    function write(ctx) {
        ctx.clearRect(0, 0, canvas_width, canvas_height);

        const text_size = measure(input[0].charAt(0), "sans-serif", fontsize);

        input.forEach((paragraph, order) => {
            ctx.fillText(paragraph, 10, order * text_size.height);
        });
        dataURL = c.toDataURL();
    }

    onMount(() => {
        ctx = c.getContext("2d");

        ctx.textBaseline = "top";
    });

    $: if (c) {
        input = input;
        write(c.getContext("2d"));
    }

    $: if (c) {
        ctx = c.getContext("2d");

        ctx.font = `${fontsize}px sans-serif`;
        write(ctx);
    }
</script>

<img src={dataURL} alt={input.join("")} />
<canvas bind:this={c} height={canvas_height} width={canvas_width} />

<span id="ruler" bind:this={target} />

<style>
    canvas {
        display: none;
    }

    #ruler {
        white-space: nowrap;
        visibility: hidden;
        overflow: hidden;
        display: inline;
    }
</style>
