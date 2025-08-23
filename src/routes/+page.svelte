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

	// New state for mobile view
	let activeView: 'wordlist' | 'generator' | 'settings' = 'generator';

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

	<div class="mobile-nav">
		<button class:active={activeView === 'wordlist'} on:click={() => (activeView = 'wordlist')}>
			Word lists
		</button>
		<button
			class="home-button"
			class:active={activeView === 'generator'}
			on:click={() => (activeView = 'generator')}
		>
			<svg
				xmlns="http://www.w3.org/2000/svg"
				height="24px"
				viewBox="0 -960 960 960"
				width="24px"
				fill="var(--text)"
				><path
					d="M160-200v-360q0-19 8.5-36t23.5-28l240-180q21-16 48-16t48 16l240 180q15 11 23.5 28t8.5 36v360q0 33-23.5 56.5T720-120H600q-17 0-28.5-11.5T560-160v-200q0-17-11.5-28.5T520-400h-80q-17 0-28.5 11.5T400-360v200q0 17-11.5 28.5T360-120H240q-33 0-56.5-23.5T160-200Z"
				/></svg
			>
		</button>
		<button class:active={activeView === 'settings'} on:click={() => (activeView = 'settings')}>
			Settings
		</button>
	</div>

	<div class="content">
		<div class="wordbar" class:show={activeView === 'wordlist'}>
			<WordSidebar bind:wordList />
		</div>

		<div class="gen" class:show={activeView === 'generator'}>
			<Generator bind:wordList {casing} {separator} />
		</div>

		<div class="gensettings" class:show={activeView === 'settings'}>
			<GenSettings bind:casing bind:separator />
		</div>
	</div>
	<div class="footer">
		made with ♥ by <a href="https://github.com/k-amryn/vibegen.io">kamryn</a>
	</div>
</main>

<style>
	main {
		padding: 10px;
		width: 100%;
		display: flex;
		flex-direction: column;
		gap: 10px;
		margin: 0 auto;
	}

	.footer {
		font-size: 0.7em;
	}

	/* Mobile navigation */
	.mobile-nav {
		display: grid;
		grid-template-columns: 1fr 50px 1fr;
		gap: 10px;
	}

	.mobile-nav button {
		flex-grow: 1;
		padding: 10px;
		border: 1px solid var(--outline);
		background: var(--surface);
		color: var(--on-surface);
		border-radius: 12px;
		font-size: 1em;
		cursor: pointer;
		display: flex;
		justify-content: center;
		align-items: center;
		transition: background-color 0.2s, border-color 0.2s;
	}

	.mobile-nav button.home-button {
		flex-grow: 0;
		flex-basis: 50px;
	}

	.mobile-nav button.active {
		background: var(--accent);
		color: var(--text);
		border-color: var(--primary);
	}

	.mobile-nav button svg {
		stroke: var(--on-surface);
		transition: stroke 0.2s;
	}

	.mobile-nav button.active svg {
		stroke: var(--on-primary-container);
	}

	/* Content area */

	.content {
		height: calc(100vh - 175px);
	}

	/* Mobile-first: hide all sections by default */
	.wordbar,
	.gen,
	.gensettings {
		display: none;
	}

	/* Show the active section */
	.wordbar.show,
	.gen.show,
	.gensettings.show {
		display: block;
		height: 100%;
	}

	.gen,
	.gensettings {
		background: var(--surface);
		border-radius: 20px;
	}

	/* Desktop layout using a media query */
	@media (min-width: 800px) {
		.mobile-nav {
			display: none; /* Hide mobile nav on desktop */
		}

		.content {
			display: flex;
			gap: 20px;
			height: calc(100vh - 120px);
		}

		/* Show all sections and set their widths */
		.wordbar,
		.gen,
		.gensettings {
			display: block; /* Override display: none */
			height: auto;
		}

		.wordbar {
			width: 25%;
		}

		.gen {
			width: 50%;
		}

		.gensettings {
			width: 25%;
		}
	}
</style>
