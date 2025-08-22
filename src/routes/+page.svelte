<script lang="ts">
	import WordSidebar from '$lib/wordSidebar.svelte';
	import Generator from '$lib/generator.svelte';
	import GenSettings from '$lib/gensettings.svelte';
	import { onMount } from 'svelte';

	let wordList: Array<Array<string>> = [[''], [''], [''], [''], ['']];
	let casing = 'unchanged';
	let separator = ' ';

	let isBrowser: boolean = typeof window == 'object';
	let isLoaded = false;

	onMount(() => decodeURL());
	$: if (isLoaded && (wordList || casing || separator)) encodeURL();

	function decodeURL() {
		const params = new URLSearchParams(window.location.search);
		// each wordlist is stored in the url as a number between 0 and 4
		for (let i = 0; i < 5; i++) {
			let e = params.get(i.toString());
			if (e != null) wordList[i] = e.split('~~');
		}
		const caseParam = params.get('case');
		if (caseParam != null) casing = caseParam;
		const sepParam = params.get('sep');
		if (sepParam != null) separator = sepParam;
		isLoaded = true;
	}

	function encodeURL() {
		let urlString = '';
		let isFirst = true;
		for (let i = 0; i < 5; i++) {
			let li = wordList[i];
			if (li.length == 1 && li[0] == '') continue;
			urlString += isFirst ? '?' : '&';
			isFirst = false;
			urlString += `${i}=${encodeURIComponent(li.join('~~'))}`;
		}
		if (casing != 'unchanged') {
			urlString += isFirst ? '?' : '&';
			urlString += `case=${casing}`;
		}
		if (separator != ' ') {
			urlString += isFirst ? '?' : '&';
			urlString += `sep=${encodeURIComponent(separator)}`;
		}
		if (isBrowser) history.replaceState(null, '', `${window.location.origin}${urlString}`);
	}
</script>

<main>
	<div class="header">
		<h1>vibegen.io</h1>
		<p>Generate random phrases from word lists.</p>
	</div>
	<div class="content">
		<div class="wordbar">
			<WordSidebar bind:wordList />
		</div>

		<div class="gen">
			<Generator bind:wordList bind:casing bind:separator />
		</div>

		<div class="gensettings">
			<GenSettings bind:casing bind:separator />
		</div>
	</div>
	<div class="footer">
		made with ♥ by <a href="https://github.com/k-amryn/vibegen.io">kamryn</a>
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
	.wordbar,
	.gensettings {
		width: 25%;
	}
	.gen,
	.gensettings {
		background: var(--surface);
		border-radius: 20px;
	}
	.content {
		width: 100%;
		height: 700px;
		display: flex;
		justify-content: space-between;
		gap: 20px;
	}
	.footer {
		font-size: 0.7em;
	}
</style>
