<script>
    import Header from "./Header.svelte";

    let input_text;
</script>

<Header />

<main>
    <section class="input_area">
        <textarea bind:value={input_text} autocomplete="off" />
    </section>
    <p>
        {input_text}
    </p>
</main>

<style>
    main {
        margin-top: 3rem;
    }

    .input_area {
        display: flex;
        width: 100%;
        padding-top: 1rem;
        flex-direction: column;
        align-items: center;
    }

    textarea {
        width: 90vw;
        height: 100px;
        resize: vertical;
    }
</style>
