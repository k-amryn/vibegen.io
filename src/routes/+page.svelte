<script lang="ts">
    import Select from '../lib/select.svelte'
    import WordSidebar from "$lib/wordSidebar.svelte"

    let wordList: Array<Array<String>>

</script>

<main>
    <div class="header">
        <h1>vibegen.io</h1> 
        <p>Generate random phrases from word lists.</p>
    </div>
    <div class="content">
        <WordSidebar bind:wordList/>

        <div class="vibes">
            <div class="gen-vibe">Generate vibe</div>
            <div class="x5">x5</div>
            <div class="x10">x10!</div>
        </div>
        
        <div class="settings">
            <p>Separator:</p>
            <div class="indent">
                <p>None</p>
                <p>Space</p>
                <p>Custom: <input></p>
            </div>
            <p>Casing:</p>
            <div class="indent">
                <p>Unchanged</p>
                <p>camelCase</p>
                <p>PascalCase</p>
            </div>
            <p>Share a link to this generator!</p>
            <div class="copy-link">Copy link</div>
        </div>
    </div>

</main>

<style>
    main {
        padding-top: 20px;
        width: 100%;
        display: flex;
        flex-direction: column;
        gap: 20px;
    }
    .vibes, .settings {
        background: white;
        border-radius: 20px;
    }
    .vibes {
        width: 50%;
    }
    .settings {
        width: 25%;
    }
    .content {
        width: 100%;
        height: 500px;
        display: flex;
        justify-content: space-between;
        gap: 20px;
    }
</style>