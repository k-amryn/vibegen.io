<script lang="ts">
	import { onMount, tick } from 'svelte';

	export let separator: string;
	export let casing: string;

	let separatorOption = 'space';
	let customSep = '';
	let copyMessageDisplayed = false;

	$: switch (separatorOption) {
		case 'space':
			separator = ' ';
			break;
		case 'custom':
			separator = customSep;
			break;
	}

	onMount(async () => {
		await tick();
		if (separator == ' ') separatorOption = 'space';
		if (separator != '' && separator != ' ') {
			separatorOption = 'custom';
			customSep = separator;
		}
	});

	function copyLink() {
		navigator.clipboard.writeText(window.location.href);
		if (copyMessageDisplayed) return;
		copyMessageDisplayed = true;
		setTimeout(() => {
			copyMessageDisplayed = false;
		}, 3000);
	}
</script>

<main>
	<p>Separator:</p>
	<div class="indent">
		<p>
			<label>
				<input type="radio" name="separator" bind:group={separatorOption} value={'space'} />
				Space
			</label>
		</p>
		<p>
			<label>
				<input type="radio" name="separator" bind:group={separatorOption} value={'custom'} />
				Custom:
			</label>
			<input
				disabled={separatorOption != 'custom'}
				class="custom-sep-input"
				bind:value={customSep}
			/>
		</p>
	</div>
	<p>Casing:</p>
	<div class="indent">
		<p>
			<label>
				<input type="radio" name="casing" bind:group={casing} value={'unchanged'} />
				Unchanged
			</label>
		</p>
		<p>
			<label>
				<input type="radio" name="casing" bind:group={casing} value={'title'} />
				TitleCase
			</label>
		</p>
		<p>
			<label>
				<input type="radio" name="casing" bind:group={casing} value={'lower'} />
				lowercase
			</label>
		</p>
		<p>
			<label>
				<input type="radio" name="casing" bind:group={casing} value={'camel'} />
				camelCase
			</label>
		</p>
	</div>
	<div class="copy-section">
		<div on:click={() => copyLink()} class="button copy-link">
			<svg
				xmlns="http://www.w3.org/2000/svg"
				height="24px"
				viewBox="0 -960 960 960"
				width="24px"
				fill="var(--text)"
				><path
					d="M280-280q-83 0-141.5-58.5T80-480q0-83 58.5-141.5T280-680h120q17 0 28.5 11.5T440-640q0 17-11.5 28.5T400-600H280q-50 0-85 35t-35 85q0 50 35 85t85 35h120q17 0 28.5 11.5T440-320q0 17-11.5 28.5T400-280H280Zm80-160q-17 0-28.5-11.5T320-480q0-17 11.5-28.5T360-520h240q17 0 28.5 11.5T640-480q0 17-11.5 28.5T600-440H360Zm200 160q-17 0-28.5-11.5T520-320q0-17 11.5-28.5T560-360h120q50 0 85-35t35-85q0-50-35-85t-85-35H560q-17 0-28.5-11.5T520-640q0-17 11.5-28.5T560-680h120q83 0 141.5 58.5T880-480q0 83-58.5 141.5T680-280H560Z"
				/></svg
			>
			Link to this generator
		</div>
		{#if copyMessageDisplayed}
			<div class="copy-message">
				<svg xmlns="http://www.w3.org/2000/svg" fill="#72DA7D" height="20" width="20"
					><path
						d="M8 15h-.5l-.4-.4-3.5-3.4q-.4-.4-.3-1 0-.5.3-.9.4-.4 1-.4t1 .4L8 11.8l6.5-6.4q.4-.4.9-.4t1 .4q.3.4.3.9t-.4 1L9 14.5l-.4.3-.5.1Z"
					/></svg
				>
				Link copied!
			</div>
		{/if}
	</div>
</main>

<style>
	main {
		background: var(--surface);
		border-radius: 20px;
		height: 100%;
		box-sizing: border-box;
		padding: 20px;
		line-height: 1.5;
	}
	.indent {
		margin-left: 20px;
		margin-bottom: 20px;
	}

	.indent p {
		margin-bottom: 12px;
	}

	main > p {
		margin-bottom: 8px;
		font-weight: 500;
	}
	.custom-sep-input {
		width: 50px;
		font-size: 1em;
		border: 2px solid black;
		border-radius: 6px;
	}
	.custom-sep-input:disabled {
		border: 2px solid var(--muted);
		color: var(--muted);
		background: var(--surface);
	}
	.copy-section {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin-top: 24px;
	}
	.copy-link {
		width: 100%;
		margin-top: 20px;
		display: flex;
		justify-content: center;
	}
	.copy-message {
		color: #72da7d;
		font-size: 0.8em;
		display: flex;
		align-items: center;
	}
</style>
