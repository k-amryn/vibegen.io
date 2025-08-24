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
		<p>Generate vibes from lists of words.</p>
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
				width="24"
				height="24"
				viewBox="0 0 19 19"
				xmlns="http://www.w3.org/2000/svg"
				class="rotor"
			>
				<path
					d="M8.61104 0.725463C8.98337 0.00278293 10.0166 0.00278307 10.389 0.725463L11.5233 2.9272C11.747 3.36145 12.2517 3.57051 12.717 3.42165L15.0759 2.66688C15.8502 2.41914 16.5809 3.14977 16.3331 3.92405L15.5784 6.28301C15.4295 6.74827 15.6386 7.25298 16.0728 7.47671L18.2745 8.61104C18.9972 8.98337 18.9972 10.0166 18.2745 10.389L16.0728 11.5233C15.6386 11.747 15.4295 12.2517 15.5784 12.717L16.3331 15.0759C16.5809 15.8502 15.8502 16.5809 15.0759 16.3331L12.717 15.5784C12.2517 15.4295 11.747 15.6386 11.5233 16.0728L10.389 18.2745C10.0166 18.9972 8.98337 18.9972 8.61104 18.2745L7.47671 16.0728C7.25298 15.6386 6.74827 15.4295 6.28301 15.5784L3.92405 16.3331C3.14977 16.5809 2.41914 15.8502 2.66688 15.0759L3.42165 12.717C3.57051 12.2517 3.36145 11.747 2.9272 11.5233L0.725463 10.389C0.00278293 10.0166 0.00278307 8.98337 0.725463 8.61104L2.9272 7.47671C3.36145 7.25298 3.57051 6.74827 3.42165 6.28301L2.66688 3.92405C2.41914 3.14977 3.14977 2.41914 3.92405 2.66688L6.28301 3.42165C6.74827 3.57051 7.25298 3.36145 7.47671 2.9272L8.61104 0.725463Z"
				/>
			</svg>
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

	@keyframes star-spin {
		from {
			transform: rotate(0deg);
		}
		to {
			transform: rotate(360deg);
		}
	}

	.mobile-nav button svg {
		stroke: var(--on-surface);
		fill: var(--text);
		transform-origin: 50% 50%;
		animation: star-spin 15s linear infinite;
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
