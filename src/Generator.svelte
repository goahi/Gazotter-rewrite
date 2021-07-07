<script>
    export let input;
    export let fontsize = 8;

    let c;
    let ctx;

    let canvas_width = 600;
    let canvas_height = 600;

    let dataURL;

    let target;
    let r_height;
    let r_width;

    if (c) {
        ctx = c.getContext("2d");

        ctx.textBaseline = "top";
    }

    $: if (c) {
        function measure(target_letters, font_family, font_size) {
            target.style.fontFamily = font_family;
            target.style.fontSize = `${font_size}px`;

            target.textContent = target_letters;

            return { height: target.offsetHeight, width: target.offsetWidth };
        }

        ctx = c.getContext("2d");

        ctx.clearRect(0, 0, canvas_width, canvas_height);
        ctx.font = `${fontsize}px sans-serif`;

        const text_height = measure(input[0], "sans-serif", fontsize).height;

        input.forEach((paragraph, order) => {
            ctx.fillText(paragraph, 10, (order + 1) * text_height);
        });
        dataURL = c.toDataURL();
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
    }
</style>
