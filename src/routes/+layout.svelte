<script>
	import Header from './Header.svelte';
	import './styles.css';
	import { onMount } from 'svelte';

	let showScreen = false;

	onMount(() => {
		showScreen = true;
	});
</script>

<div class="app">
	{#if showScreen}
		<Header />
		<main>
			<slot />
		</main>
	{/if}
</div>