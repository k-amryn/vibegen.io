<script lang="ts">
    import Select from '../lib/select.svelte'
    import WordSidebar from "$lib/wordSidebar.svelte"
    import Generator from "$lib/generator.svelte"
    import GenSettings from "$lib/gensettings.svelte"
    import { onMount } from 'svelte'

    let wordList: Array<Array<String>> = [[''], [''], [''], [''], ['']]
    let casing: String = 'unchanged'
    let separator: string = ''

    let isBrowser: boolean = typeof window == 'object'
    let isLoaded: boolean = false

    onMount(() => decodeURL())
    $: if (isLoaded && (wordList || casing || separator)) encodeURL()

    function decodeURL() {
        const params = new URLSearchParams(window.location.search);
        // each wordlist is stored in the url as a number between 0 and 4
        for (let i = 0; i < 5; i++) {
            let e = params.get(i.toString())
            if (e != null) wordList[i] = e.split('~~')
        }
        // @ts-ignore
        if (params.get('case') != null) casing = params.get('case')
        // @ts-ignore
        if (params.get('sep') != null) separator = params.get('sep')
        isLoaded = true
    }

    function encodeURL() {
        let urlString = ''
        let isFirst = true
        for (let i = 0; i<5; i++) {
            let li = wordList[i]
            if (li.length == 1 && li[0] == '') continue
            urlString += isFirst ? "?" : "&"
            isFirst = false
            urlString += `${i}=${encodeURIComponent(li.join("~~"))}`
        }
        if (casing != "unchanged") {
            urlString += isFirst ? "?" : "&"
            urlString += `case=${casing}`
        }
        if (separator != "") {
            urlString += isFirst ? "?" : "&"
            urlString += `sep=${encodeURIComponent(separator)}`
        }
        if (isBrowser) history.replaceState(null, '', `${window.location.origin}${urlString}`)
    }
</script>

<main>
    <div class="header">
        <h1>vibegen.io</h1> 
        <p>Generate random phrases from word lists.</p>
    </div>
    <div class="content">
        <div class="wordbar">
            <WordSidebar bind:wordList/>
        </div>

        <div class="gen">
            <Generator bind:wordList bind:casing bind:separator/>
        </div>
        
        <div class="gensettings">
            <GenSettings bind:casing bind:separator/>
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
    .gen {
        width: 50%;
    }
    .wordbar, .gensettings {
        width: 25%;
    }
    .gen, .gensettings {
        background: white;
        border-radius: 20px;
    }
    .content {
        width: 100%;
        height: 500px;
        display: flex;
        justify-content: space-between;
        gap: 20px;
    }
</style>