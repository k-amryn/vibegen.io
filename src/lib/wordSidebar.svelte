<script lang="ts">
    let activeList = 0
    let wordListRaw: Array<String> = [
        '',
        '',
        '',
        '',
        ''
    ]

    export let wordList: Array<Array<String>> = [[''], [''], [''], [''], ['']]

    function processInput() {
        wordList[activeList] = wordListRaw[activeList].split("\n")
    }
</script>

<main>
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
            <textarea bind:value={wordListRaw[activeList]} on:input={processInput}></textarea>
            <div class="button clear-words"
                on:click={() => {wordListRaw[activeList] = ''; wordList[activeList] = ['']}}
            >
                <svg xmlns="http://www.w3.org/2000/svg" height="20" width="20"><path d="M5.812 18.167q-.958 0-1.624-.667-.667-.667-.667-1.625V5.042q-.459 0-.802-.344-.344-.344-.344-.802 0-.458.344-.802.343-.344.802-.344h3.958q0-.417.344-.729.344-.313.781-.313h2.813q.437 0 .781.313.344.312.344.729H16.5q.458 0 .802.344.344.344.344.802 0 .458-.344.802-.344.344-.802.344v10.833q0 .958-.667 1.625-.666.667-1.625.667ZM7 12.958q0 .459.344.802.344.344.802.344.458 0 .802-.344.344-.343.344-.802v-5q0-.458-.344-.802-.344-.344-.802-.344-.458 0-.802.344Q7 7.5 7 7.958Zm3.708 0q0 .459.344.802.344.344.802.344.458 0 .802-.344.344-.343.344-.802v-5q0-.458-.344-.802-.344-.344-.802-.344-.458 0-.802.344-.344.344-.344.802Z"/></svg>
                Clear list
            </div>
        </div>
    </div>
</main>

<style>
    textarea {
        font-family: JetBrainsMono, monospace;
    }
    main {
        display: flex;
        flex-direction: column;
        height: 100%;
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
        padding: 20px 20px 10px 20px;
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
</style>