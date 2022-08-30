<script lang="ts">
    import Select from '../lib/select.svelte'
    import WordSidebar from "$lib/wordSidebar.svelte"
    import Generator from "$lib/generator.svelte"
    import GenSettings from "$lib/gensettings.svelte"

    let wordList: Array<Array<String>>
    let casing: String = 'unchanged'
    let separator: String = ''

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