<script>
    import { createEventDispatcher } from "svelte";

    let font_size = 8;

    const dispatch = createEventDispatcher();

    dispatch("text_setting", { fontsize: font_size });

    $: dispatch("text_setting", { fontsize: font_size });
</script>

<section class="text_setting">
    <div>
        フォントサイズ
        <input class="font_size" type="number" min="1" bind:value={font_size} />
    </div>
</section>

<style>
    .text_setting {
        display: flex;
    }

    .font_size {
        font-size: 1.25em;
        height: 1.5em;
        width: 6ch;
    }
</style>
