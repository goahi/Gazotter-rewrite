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
        margin-top: 3rem;
        min-height: calc(100vh - 3rem);

        display: flex;
        align-items: center;
        justify-content: space-around;
    }

    .input_area {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    textarea {
        resize: none;
    }

    @media screen and (min-width: 600px) {
        main {
            flex-direction: row;
        }
        .input_area {
            width: 40vw;
            height: calc(90vh - 3rem);
        }
        textarea {
            width: 100%;
            height: 100%;
        }

        .preview {
            width: 40vw;
            height: calc(90vh - 3rem);
        }
    }
    @media screen and (max-width: 599px) {
        main {
            flex-direction: column;
        }
        .input_area {
            width: 90vw;
            height: 40vh;
        }
        textarea {
            width: 100%;
            height: 100%;
        }

        .preview {
            width: 90vw;
            height: 40vh;
        }
    }
</style>
