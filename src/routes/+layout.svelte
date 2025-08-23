<script lang="ts">
	import '../global.css';
	import { onMount } from 'svelte';

	onMount(() => {
		// Listen for OS theme changes to ensure CSS media query stays responsive
		if (window.matchMedia) {
			const mediaQuery = window.matchMedia('(prefers-color-scheme: dark)');
			const handleChange = () => {
				// CSS media query automatically handles the visual theme change
				// This listener just ensures the media query stays active
			};

			mediaQuery.addEventListener('change', handleChange);

			// Cleanup listener on component destroy
			return () => {
				mediaQuery.removeEventListener('change', handleChange);
			};
		}
	});
</script>

<main>
	<slot />
</main>

<style>
	main {
		height: 100vh;
		max-width: 1500px;
		padding: 10px;
		box-sizing: border-box;
		margin: auto;
	}
</style>
