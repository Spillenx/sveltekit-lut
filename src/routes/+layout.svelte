<script>
	import './styles.css';
	import { env } from '$env/dynamic/public';
	import { PUBLIC_LUT_PUB_KEY } from '$env/static/public';
	import { browser, building, dev, version } from '$app/environment';

	// lifecycle, afterNavigate, beforeNavigate
	// disableScrollHandling -> SvetlteKit's built in scroll handling
	// goto -> programitacally routing
	// invalidate -> load fucntions on active page rerun
	// invalidateAll -> causes all load functions to rerun
	// preloadCode -> load js code for given routes
	// preloadData -> load data from load function from given routes
	import { goto } from '$app/navigation';

	import { navigating } from '$app/stores';
	import Header from './Header.svelte';
	import Footer from './Footer.svelte';
	import Episodes from './Episodes.svelte';

	export let data;
	$: ({ all_episodes } = data);

	// browser -> boolean, app is running in broswer
	// dev -> boolean, if on development mode
	// building -> boolean, if building for production
	// version -> config.kit.version.name
	console.log(browser, building, dev, version);
</script>

<Header />

{#if !!$navigating}
	<div class="loading">Loading...</div>
{/if}

<main>
	<div class="main">
		<button on:click={() => goto('/contact')}>Change Page</button>
		<slot />
	</div>
	<aside>
		<Episodes episodes={all_episodes} />
	</aside>
</main>

<Footer />

<style lang="postcss">
	main {
		display: grid;
		grid-template-columns: 300px 1fr;
		gap: 20px;
	}

	aside {
		order: -1;
	}

	.loading {
		position: fixed;
		inset: 0;
		display: flex;
		justify-content: center;
		align-items: center;
		background: blue;
		color: white;
	}
</style>
