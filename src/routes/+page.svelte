<script>
    import Select from '../lib/select.svelte'
    let activeList = 0
    $: activeListWords = wordList[activeList].join('\n')
    let wordList = []

</script>

<main>
    <div class="header">
        <h1>vibegen.io</h1> 
        <p>Generate random phrases from word lists.</p>
    </div>
    <div class="content">
        <div class="wordbar">
            <div class="wordbar-tabs">
                <span on:click={() => activeList = 0}
                    class:active="{activeList == 0}"
                    class="word-tab one"
                    >1</span>
                <span on:click={() => activeList = 1}
                    class:active="{activeList == 1}"
                    class="word-tab two"
                    >2</span>
                <span on:click={() => activeList = 2}
                    class:active="{activeList == 2}"
                    class="word-tab three"
                    >3</span>
                <span on:click={() => activeList = 3}
                    class:active="{activeList == 3}"
                    class="word-tab four"
                    >4</span>
                <span on:click={() => activeList = 4}
                    class:active="{activeList == 4}"
                    class="word-tab five"
                    >5</span>
            </div>
            <div class="wordlist-wrapper">
                <div class="wordlist">
                    <div class="word-settings">
                        <span>Nouns</span>
                        <span>Adjectives</span>
                    </div>
                    <textarea>{activeListWords}</textarea>
                    <div class="clear-words">Clear list</div>
                </div>
            </div>
        </div>

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
    .wordbar, .settings {
        width: 25%;
    }
    .vibes {
        width: 50%;
    }
    .wordbar {
        display: flex;
        flex-direction: column;
    }
    .wordbar-tabs {
        height: 45px;
        display: flex;
        justify-content: space-around;
        align-items: center;
    }
    .word-tab {
        height: 100%;
        width: 100%;
        display: grid;
        place-content: center;
        cursor: pointer;
        -webkit-user-select: none;
        user-select: none;
    }
    .word-tab.active {
        background: white;
        border-radius: 10px 10px 0px 0px;
        position: relative;
    }
    .word-tab.active:not(.one)::before {
        content: '';
        position: absolute;
        height: 10px;
        width: 10px;
        left: -10px;
        bottom: 0px;
        background: transparent;
        display: inline;
        border-bottom-right-radius: 8px;
        box-shadow: 4px 4px 0px 0px white;
    }
    .word-tab.active.one::before {
        content: '';
        position: absolute;
        background: white;
        height: 20px;
        width: 20px;
        bottom: -20px;
        z-index: 1;
    }
    .word-tab.active:not(.five)::after {
        content: '';
        position: absolute;
        height: 10px;
        width: 10px;
        right: -10px;
        bottom: 0px;
        z-index: 10;
        background: transparent;
        display: inline;
        border-bottom-left-radius: 8px;
        box-shadow: -4px 4px 0px 0px white;
    }
    .word-tab.active.five::after {
        content: '';
        position: absolute;
        background: white;
        height: 20px;
        width: 20px;
        right: 0;
        bottom: -20px;
    }
    .wordlist-wrapper {
        background: white;
        border-radius: 20px 20px 20px 20px;
        height: 100%;
    }
    .wordlist {
        position: relative;
        z-index: 2;
        padding: 10px 20px;
        box-sizing: border-box;
        display: flex;
        flex-direction: column;
        align-items: center;
        align-content: space-between;
        justify-content: space-between;
        gap: 10px;
        height: 100%;
    }
    .wordlist textarea {
        resize: none;
        height: 100%;
        width: 100%;
        padding: 10px;
        box-sizing: border-box;
        border: 2px solid black;
        border-radius: 10px;
    }
    .content {
        width: 100%;
        height: 500px;
        display: flex;
        justify-content: space-between;
        gap: 20px;
    }
</style>