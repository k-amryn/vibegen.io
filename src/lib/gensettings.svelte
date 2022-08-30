<script lang="ts">
    export let separator: String
    export let casing: String = 'unchanged'

    let separatorOption: String = 'none'
    let customSep: String = ''
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
    <div class="button copy-link">Link to this generator!</div>
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
    .copy-link {
        margin-top: 20px;
        display: flex;
        justify-content: center;
    }
</style>