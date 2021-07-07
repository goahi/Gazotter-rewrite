<script>
    import { createEventDispatcher } from "svelte";

    let font_size = 8;
    let line_spacing = 1.1;

    const dispatch = createEventDispatcher();

    dispatch("text_setting", {
        fontsize: font_size,
        linespacing: line_spacing,
    });

    $: dispatch("text_setting", {
        fontsize: font_size,
        linespacing: line_spacing,
    });
</script>

<section class="text_setting">
    <div>
        フォントサイズ
        <input
            class="inputbox_num"
            type="number"
            min="1"
            max="160"
            bind:value={font_size}
        />
        行間
        <input
            class="inputbox_num"
            type="number"
            min="1"
            max="5"
            step="0.1"
            bind:value={line_spacing}
        />
    </div>
</section>

<style>
    .text_setting {
        display: flex;
    }

    .inputbox_num {
        font-size: 1.25em;
        height: 1.5em;
        width: 6ch;
    }
</style>
