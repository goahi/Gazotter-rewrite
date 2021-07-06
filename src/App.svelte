<script>
    import Header from "./Header.svelte";

    let input_text = "";
    let input_html;
    $: input_html = input_text.split("\n");
</script>

<Header />

<main>
    <section class="input_area">
        <textarea bind:value={input_text} autocomplete="off" />
    </section>
    <section class="preview">
        {#each input_html as paragraph}
            {paragraph}
            <br />
        {/each}
    </section>
</main>

<style>
    main {
        display: flex;

        justify-content: space-around;
        margin-top: 3rem;
    }

    .input_area {
        display: flex;
        width: 45vw;
        padding-top: 1rem;
        flex-direction: column;
        align-items: center;
    }

    textarea {
        width: 100%;
        height: 120px;
        resize: vertical;
    }

    .preview {
        width: 45vw;
    }
</style>
