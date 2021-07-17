<script>
    import Header from "./Header.svelte";
    import Generator from "./Generator.svelte";
    import TextSetting from "./TextSetting.svelte";

    let textsetting;
    let autosave_interval = 15;

    const stored_text = localStorage.getItem("text");
    let input_text = stored_text === null ? "" : stored_text;
    let input_html;
    $: input_html = input_text.split("\n");

    function get_textsetting(event) {
        textsetting = event.detail;
    }

    window.onbeforeunload = function save_text() {
        localStorage.setItem("text", input_text);
    };

    setInterval(() => {
        localStorage.setItem("text", input_text);
    }, autosave_interval * 1000);
</script>

<Header />

<main>
    <section class="input_area">
        <TextSetting on:text_setting={get_textsetting} />
        <textarea bind:value={input_text} autocomplete="off" />
    </section>
    <section class="preview">
        <Generator input={input_html} {...textsetting} />
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
