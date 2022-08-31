<script lang="ts">
    import { onMount, tick } from 'svelte'

    export let separator: String
    export let casing: String

    let separatorOption: String = 'none'
    let customSep: String = ''
    let copyMessageDisplayed: boolean = false

    $: switch (separatorOption) {
        case 'none':
            separator = ''
            break;
        case 'space':
            separator = ' '
            break
        case 'custom':
            separator = customSep
            break
    }

    onMount(async () => {
        await tick()
        if (separator == " ") separatorOption = 'space'
        if (separator != "" && separator != " ") {
            separatorOption = 'custom'
            customSep = separator
        }
    })

    function copyLink() {
        navigator.clipboard.writeText(window.location.href)
        if (copyMessageDisplayed) return
        copyMessageDisplayed = true
        setTimeout(() => {copyMessageDisplayed = false}, 3000);
    }
</script>

<main>
    <p>Separator:</p>
    <div class="indent">
        <p>
            <label>
                <input type="radio" name="separator"
                    bind:group={separatorOption} value={'none'}
                >
                None
            </label>
        </p>
        <p>
            <label>
                <input type="radio" name="separator"
                    bind:group={separatorOption} value={'space'}
                >
                Space
            </label>
        </p>
        <p>
            <label>
                <input type="radio" name="separator"
                    bind:group={separatorOption} value={'custom'}
                >
                Custom:
            </label>
            <input disabled={separatorOption!='custom'} class="custom-sep-input" bind:value={customSep}>
        </p>
    </div>
    <p>Casing:</p>
    <div class="indent">
        <p>
            <label>
                <input type="radio" name="casing"
                    bind:group={casing} value={'unchanged'}
                >
                Unchanged
            </label>
        </p>
        <p>
            <label>
                <input type="radio" name="casing"
                    bind:group={casing} value={'title'}
                >
                TitleCase
            </label>
        </p>
        <p>
            <label>
                <input type="radio" name="casing"
                    bind:group={casing} value={'lower'}
                >
                lowercase
            </label>
        </p>
        <p>
            <label>
                <input type="radio" name="casing"
                    bind:group={casing} value={'camel'}
                >
                camelCase
            </label>
        </p>
    </div>
    <div class="copy-section">
        <div on:click={() => copyLink()} class="button copy-link">
            Link to this generator!
        </div>
        {#if copyMessageDisplayed}
            <div class="copy-message">
                <svg xmlns="http://www.w3.org/2000/svg" fill="#72DA7D" height="20" width="20"><path d="M8 15h-.5l-.4-.4-3.5-3.4q-.4-.4-.3-1 0-.5.3-.9.4-.4 1-.4t1 .4L8 11.8l6.5-6.4q.4-.4.9-.4t1 .4q.3.4.3.9t-.4 1L9 14.5l-.4.3-.5.1Z"/></svg>
                Link copied!
            </div>
        {/if}
    </div>
</main>

<style>
    main {
        background: white;
        border-radius: 20px;
        height: 100%;
        box-sizing: border-box;
        padding: 10px;
    }
    .indent {
        margin-left: 20px;
    }
    .custom-sep-input {
        width: 50px;
        font-size: 1em;
        border: 2px solid black;
        border-radius: 6px;
    }
    .custom-sep-input:disabled {
        border: 2px solid grey;
        color: grey;
        background: white;
    }
    .copy-section {
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    .copy-link {
        width: 100%;
        margin-top: 20px;
        display: flex;
        justify-content: center;
    }
    .copy-message {
        color: #72DA7D;
        font-size: 0.8em;
        display: flex;
        align-items: center;
    }
</style>