<script lang="ts">
	export let wordList: Array<Array<string>>;
	export let casing: string;
	export let separator: string;

	let vibesProcessed: Array<string> = ['Vibes Appear Here'];

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
			vibesProcessed.unshift(currentVibe);
			vibesProcessed = vibesProcessed;
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
		<div on:click={() => genVibe(1)} class="button gen-vibe">Generate vibe</div>

		<div class="mods">
			<div on:click={() => genVibe(5)} class="x5">
				<svg
					width="50"
					height="50"
					viewBox="0 0 23 23"
					fill="none"
					xmlns="http://www.w3.org/2000/svg"
					><path
						d="m11.5 0 1.6 2.99L15.64.78l.4 3.36L19.26 3l-.84 3.28 3.38.1-1.96 2.75 3.12 1.3-2.83 1.87 2.44 2.35-3.3.71 1.42 3.07-3.35-.53.22 3.38-2.92-1.7-1.02 3.22-2.11-2.64-2.11 2.64-1.02-3.23-2.92 1.7.22-3.37-3.35.53 1.43-3.07-3.31-.71 2.44-2.35-2.83-1.86 3.12-1.31-1.96-2.76 3.38-.09L3.75 3l3.2 1.14.4-3.36 2.56 2.2L11.5 0Z"
						fill="#F6B980"
					/></svg
				>
				<span>x5</span>
			</div>
			<div on:click={() => genVibe(10)} class="x10">
				<svg
					width="50"
					height="50"
					viewBox="0 0 23 23"
					fill="none"
					xmlns="http://www.w3.org/2000/svg"
					><path
						d="m11.5 0 1.6 2.99L15.64.78l.4 3.36L19.26 3l-.84 3.28 3.38.1-1.96 2.75 3.12 1.3-2.83 1.87 2.44 2.35-3.3.71 1.42 3.07-3.35-.53.22 3.38-2.92-1.7-1.02 3.22-2.11-2.64-2.11 2.64-1.02-3.23-2.92 1.7.22-3.37-3.35.53 1.43-3.07-3.31-.71 2.44-2.35-2.83-1.86 3.12-1.31-1.96-2.76 3.38-.09L3.75 3l3.2 1.14.4-3.36 2.56 2.2L11.5 0Z"
						fill="#F78780"
					/></svg
				>
				<span>x10!</span>
			</div>
		</div>
	</div>

	<div class="vibes">
		{#each vibesProcessed as vibe}
			<p>{vibe}</p>
		{/each}
	</div>
</main>

<style>
	main {
		background: var(--surface);
		border-radius: 20px;
		height: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		overflow: auto;
		position: relative;
		padding-top: 10px;
		box-sizing: border-box;
	}
	.top-bar {
		display: flex;
		justify-content: center;
		align-items: center;
		padding: 10px;
		border: 2px solid var(--border);
		width: 90%;
		top: 0;
		position: sticky;
		background: var(--surface);
		border-radius: 15px;
	}
	.gen-vibe {
		padding: 10px;
	}
	.mods {
		width: 0px;
		height: 100%;
		right: -50px;
		display: flex;
	}
	.x5,
	.x10 {
		position: relative;
		display: flex;
		align-content: center;
		justify-content: center;
		user-select: none;
		-webkit-user-select: none;
		cursor: pointer;
	}
	.mods span {
		position: absolute;
		top: 12px;
		transform: rotateZ(-20deg);
	}
	.vibes {
		display: flex;
		flex-direction: column;
		align-items: center;
		height: 100%;
		width: 100%;
	}
</style>
