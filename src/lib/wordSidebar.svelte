<script lang="ts">
	import { onMount, tick } from 'svelte';

	let activeList = 0;
	let wordListRaw: Array<string> = [];
	let textareaElement: HTMLTextAreaElement;
	let presets: Record<string, string> = {
		Adjectives: 'adjectives',
		'Everyday Nouns': 'everyday_nouns',
		'Obscure Nouns': 'obscure_nouns',
		'Musical Keys': 'musical_keys',
		'Musical Instruments': 'instruments',
		'Audio Processing': 'audio_processing'
	};
	let selectedPreset = '';

	export let wordList: Array<Array<string>>;

	$: currentText = wordListRaw[activeList] || '';
	$: lineCount = currentText.split('\n').length;
	$: lineNumbers = Array.from({ length: Math.max(lineCount, 1) }, (_, i) => i + 1);

	onMount(async () => {
		await tick();
		wordList.forEach((e, i) => (wordListRaw[i] = e.join('\n')));
	});

	function processInput() {
		wordList[activeList] = wordListRaw[activeList].split('\n');
	}

	function handleScroll() {
		const lineNumbersEl = document.querySelector(`#line-numbers-${activeList}`);
		if (lineNumbersEl && textareaElement) {
			lineNumbersEl.scrollTop = textareaElement.scrollTop;
		}
	}

	async function applyPreset() {
		if (selectedPreset && presets[selectedPreset]) {
			try {
				const response = await fetch(`/presets/${presets[selectedPreset]}.json`);
				const presetItems: string[] = await response.json();

				// Append preset items without duplicating existing entries
				const existing = wordListRaw[activeList].split('\n').filter((l) => l.trim() !== '');
				const toAdd = presetItems.filter((item) => !existing.includes(item));
				wordListRaw[activeList] = [...existing, ...toAdd].join('\n');
				processInput();
			} catch (e) {
				console.error(`Failed to load preset: ${selectedPreset}`, e);
			} finally {
				// Reset dropdown to placeholder
				selectedPreset = '';
			}
		}
	}
</script>

<main>
	<div class="wordbar-tabs">
		<span on:click={() => (activeList = 0)} class:active={activeList == 0} class="word-tab one"
			>1</span
		>
		<span on:click={() => (activeList = 1)} class:active={activeList == 1} class="word-tab two"
			>2</span
		>
		<span on:click={() => (activeList = 2)} class:active={activeList == 2} class="word-tab three"
			>3</span
		>
		<span on:click={() => (activeList = 3)} class:active={activeList == 3} class="word-tab four"
			>4</span
		>
		<span on:click={() => (activeList = 4)} class:active={activeList == 4} class="word-tab five"
			>5</span
		>
	</div>
	<div class="wordlist-wrapper">
		<div class="wordlist">
			<div class="editor-container">
				<div class="line-numbers" id="line-numbers-{activeList}">
					{#each lineNumbers as lineNum}
						<div class="line-number">{lineNum}</div>
					{/each}
				</div>
				<textarea
					bind:this={textareaElement}
					bind:value={wordListRaw[activeList]}
					on:input={processInput}
					on:scroll={handleScroll}
					spellcheck="false"
					placeholder="One word per line, or choose a preset below."
				/>
			</div>
			<div class="presets-dropdown">
				<select bind:value={selectedPreset} on:change={applyPreset}>
					<option value="">Preset word lists</option>
					{#each Object.keys(presets) as preset}
						<option value={preset}>{preset}</option>
					{/each}
				</select>
			</div>
			<div
				class="button clear-words"
				on:click={() => {
					wordListRaw[activeList] = '';
					wordList[activeList] = [''];
				}}
			>
				<svg xmlns="http://www.w3.org/2000/svg" height="20" width="20"
					><path
						d="M5.812 18.167q-.958 0-1.624-.667-.667-.667-.667-1.625V5.042q-.459 0-.802-.344-.344-.344-.344-.802 0-.458.344-.802.343-.344.802-.344h3.958q0-.417.344-.729.344-.313.781-.313h2.813q.437 0 .781.313.344.312.344.729H16.5q.458 0 .802.344.344.344.344.802 0 .458-.344.802-.344.344-.802.344v10.833q0 .958-.667 1.625-.666.667-1.625.667ZM7 12.958q0 .459.344.802.344.344.802.344.458 0 .802-.344.344-.343.344-.802v-5q0-.458-.344-.802-.344-.344-.802-.344-.458 0-.802.344Q7 7.5 7 7.958Zm3.708 0q0 .459.344.802.344.344.802.344.458 0 .802-.344.344-.343.344-.802v-5q0-.458-.344-.802-.344-.344-.802-.344-.458 0-.802.344-.344.344-.344.802Z"
					/></svg
				>
				Clear list
			</div>
		</div>
	</div>
</main>

<style>
	textarea {
		font-family: JetBrainsMono, monospace;
		font-size: 0.8em;
	}
	main {
		display: flex;
		flex-direction: column;
		height: 100%;
		min-height: 0;
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
		background: var(--surface);
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
		box-shadow: 4px 4px 0px 0px var(--surface);
	}
	.word-tab.active.one::before {
		content: '';
		position: absolute;
		background: var(--surface);
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
		box-shadow: -4px 4px 0px 0px var(--surface);
	}
	.word-tab.active.five::after {
		content: '';
		position: absolute;
		background: var(--surface);
		height: 20px;
		width: 20px;
		right: 0;
		bottom: -20px;
	}
	.wordlist-wrapper {
		background: var(--surface);
		border-radius: 20px 20px 20px 20px;
		flex: 1;
		overflow-y: auto;
		min-height: 0;
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
	.editor-container {
		display: flex;
		height: 100%;
		width: 100%;
		border: 2px solid black;
		border-radius: 10px;
		overflow: hidden;
		background: var(--surface);
	}

	.editor-container textarea {
		scrollbar-width: thin;
	}

	.line-numbers {
		background: var(--button-bg);
		color: var(--muted);
		font-family: JetBrainsMono, monospace;
		font-size: 0.8em;
		line-height: 1.2;
		padding: 10px 8px 10px 10px;
		border-right: 1px solid var(--border);
		overflow: hidden;
		user-select: none;
		text-align: right;
		min-width: 30px;
	}

	.line-number {
		height: 1.2em;
		display: flex;
		align-items: center;
		justify-content: flex-end;
	}

	.wordlist textarea {
		resize: none;
		height: 100%;
		flex: 1;
		padding: 10px;
		box-sizing: border-box;
		border: none;
		border-radius: 0;
		outline: none;
		background: transparent;
	}
	textarea {
		font-family: JetBrainsMono, monospace;
		font-size: 0.8em;
		line-height: 1.2;
		color: var(--text);
	}
	.presets-dropdown {
		width: 100%;
		display: flex;
		flex-direction: column;
		gap: 5px;
	}

	.presets-dropdown select {
		padding: 6px 8px;
		border-radius: 6px;
		border: 1px solid var(--border);
		background: var(--surface);
		color: var(--text);
		font-family: inherit;
	}

	svg {
		fill: var(--text);
	}
</style>
