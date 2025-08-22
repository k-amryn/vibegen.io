<script lang="ts">
	import { slide } from 'svelte/transition';

	export let wordList: Array<Array<string>>;
	export let casing: string;
	export let separator: string;

	let vibesProcessed: Array<{ id: string; text: string }> = [];
	// qty is the number of vibes being generated, i.e. from the x5 or x10 button
	function genVibe(qty: number) {
		let newVibes: Array<Array<string>> = [];
		for (let i = 0; i < qty; i++) {
			let thisVibe: Array<string> = [];

			// 5 total lists, get random word from each list
			for (let listIndex = 0; listIndex < 5; listIndex++) {
				let currentList = wordList[listIndex];
				if (currentList.length === 1 && currentList[0] === '') {
					continue; // skip list if empty
				}
				thisVibe.push(currentList[randIndex(currentList.length)]);
			}
			newVibes.unshift(thisVibe);
		}
		processVibes(newVibes);
	}

	function processVibes(vibesRaw: Array<Array<string>>) {
		vibesRaw.forEach((vibe) => {
			let currentVibe = '';
			vibe.forEach((word, i) => {
				word = chooseCase(word, i);
				if (i < vibe.length - 1) word = `${word}${separator}`;
				currentVibe = `${currentVibe}${word}`;
			});
			const id = Date.now().toString() + Math.random().toString(36).slice(2);
			vibesProcessed.unshift({ id, text: currentVibe });
			vibesProcessed = [...vibesProcessed];
		});
	}

	function randIndex(length: number) {
		return Math.floor(Math.random() * length);
	}

	function chooseCase(vibeword: string, index: number): string {
		if ((casing == 'camel' && index == 0) || casing == 'lower') {
			return vibeword.charAt(0).toLowerCase() + vibeword.substring(1);
		} else if (casing == 'title' || casing == 'camel') {
			return vibeword.charAt(0).toUpperCase() + vibeword.substring(1);
		}
		return vibeword;
	}
</script>

<main>
	<div class="top-bar">
		<div on:click={() => genVibe(1)} class="button gen-vibe">
			<svg
				xmlns="http://www.w3.org/2000/svg"
				height="24px"
				viewBox="0 -960 960 960"
				width="24px"
				fill="var(--text)"
				><path
					d="M346-634 226-754l51-51 120 120-51 51Zm134-85v-170h72v170h-72Zm274 493L634-346l51-51 120 120-51 51Zm-57-420-51-51 120-120 51 51-120 120Zm22 166v-72h170v72H719ZM220-102 102-221q-11-10.64-11-24.82t11.3-25.45L444-613q34.79-35 84.5-35t84.5 35q35 34.79 35 84.5T613-444L271.27-102.3Q260-91 245.45-91q-14.54 0-25.45-11Zm268-318 74-75q14-14.45 14-33.73Q576-548 562-562q-14-14-33.27-14-19.28 0-33.73 14l-75 74 68 68Z"
				/></svg
			>
			Generate vibe
		</div>
	</div>

	<div class="vibes">
		{#if vibesProcessed.length === 0}
			<div class="central-star-container">
				<div class="star-wrap" aria-hidden="true">
					<svg
						width="120"
						height="120"
						viewBox="0 0 19 19"
						xmlns="http://www.w3.org/2000/svg"
						class="rotor"
					>
						<path
							d="M8.61104 0.725463C8.98337 0.00278293 10.0166 0.00278307 10.389 0.725463L11.5233 2.9272C11.747 3.36145 12.2517 3.57051 12.717 3.42165L15.0759 2.66688C15.8502 2.41914 16.5809 3.14977 16.3331 3.92405L15.5784 6.28301C15.4295 6.74827 15.6386 7.25298 16.0728 7.47671L18.2745 8.61104C18.9972 8.98337 18.9972 10.0166 18.2745 10.389L16.0728 11.5233C15.6386 11.747 15.4295 12.2517 15.5784 12.717L16.3331 15.0759C16.5809 15.8502 15.8502 16.5809 15.0759 16.3331L12.717 15.5784C12.2517 15.4295 11.747 15.6386 11.5233 16.0728L10.389 18.2745C10.0166 18.9972 8.98337 18.9972 8.61104 18.2745L7.47671 16.0728C7.25298 15.6386 6.74827 15.4295 6.28301 15.5784L3.92405 16.3331C3.14977 16.5809 2.41914 15.8502 2.66688 15.0759L3.42165 12.717C3.57051 12.2517 3.36145 11.747 2.9272 11.5233L0.725463 10.389C0.00278293 10.0166 0.00278307 8.98337 0.725463 8.61104L2.9272 7.47671C3.36145 7.25298 3.57051 6.74827 3.42165 6.28301L2.66688 3.92405C2.41914 3.14977 3.14977 2.41914 3.92405 2.66688L6.28301 3.42165C6.74827 3.57051 7.25298 3.36145 7.47671 2.9272L8.61104 0.725463Z"
						/>
					</svg>
					<span class="star-label">No vibes yet</span>
				</div>
			</div>
		{:else}
			<div class="vibe-list">
				{#each vibesProcessed as vibe (vibe.id)}
					<p in:slide={{ duration: 300 }}>{vibe.text}</p>
				{/each}
			</div>
		{/if}
	</div>
</main>

<style>
	main {
		background: var(--surface);
		border-radius: 20px;
		height: 100%;
		display: flex;
		flex-direction: column;
		position: relative;
		padding: 15px 15px 0px 15px;
		box-sizing: border-box;
	}
	.top-bar {
		display: flex;
		justify-content: center;
		align-items: center;
		padding: 10px;
		border: 2px solid var(--border);
		width: 100%;
		top: 0;
		position: sticky;
		background: var(--surface);
		border-radius: 15px;
	}
	.gen-vibe {
		padding: 10px;
	}
	.vibes {
		display: flex;
		flex-direction: column;
		align-items: center;
		text-align: center;
		line-height: 2em;
		flex: 1;
		width: 100%;
		overflow: auto;
	}

	.central-star-container {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 100%;
		width: 100%;
	}

	@keyframes star-spin {
		from {
			transform: rotate(0deg);
		}
		to {
			transform: rotate(360deg);
		}
	}
	.star-wrap {
		position: relative;
		width: 200px;
		height: 200px;
		display: inline-block;
	}
	.star-wrap svg {
		display: block;
		width: 100%;
		height: 100%;
		fill: rgba(0, 0, 0, 0.2);
	}
	.rotor {
		transform-origin: 50% 50%;
		animation: star-spin 30s linear infinite;
	}
	.star-label {
		position: absolute;
		inset: 0;
		display: flex;
		align-items: center;
		justify-content: center;
		font-weight: 700;
		color: rgba(127, 127, 127, 0.4);
		pointer-events: none;
		transform: translateZ(0);
	}
	@keyframes star-spin {
		from {
			transform: rotate(0deg);
		}
		to {
			transform: rotate(360deg);
		}
	}
</style>
