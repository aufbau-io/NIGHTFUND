<script>
	import './app.css';

	import { onMount } from 'svelte';
	import { screenType, iframe } from '$lib/store/store';

	import Header from '$lib/components/header/header.svelte';
	import Footer from '$lib/components/footer/footer.svelte';

	let Geometry;
	onMount(async () => {

		const module = await import('$lib/components/three/nightfund.svelte');
		Geometry = module.default;

		// ---------------------------------------------------------------------------
		// HEIGHT
		// ---------------------------------------------------------------------------

		let vh = window.innerHeight * 0.01;
		document.documentElement.style.setProperty('--vh', `${vh}px`);

		window.addEventListener('resize', () => {
			let vh = window.innerHeight * 0.01;
			document.documentElement.style.setProperty('--vh', `${vh}px`);
		});

		// ---------------------------------------------------------------------------
		// SCREEN
		// ---------------------------------------------------------------------------
		const ua = navigator.userAgent;
		if (
			/Mobile|Android|iP(hone|od)|IEMobile|BlackBerry|Kindle|Silk-Accelerated|(hpw|web)OS|Opera M(obi|ini)/.test(ua)
		) {
			// phone
			screenType.set(1);
		} else if (/(tablet|ipad|playbook|silk)|(android(?!.*mobi))/i.test(ua)) {
			// tablet
			screenType.set(2);
		} else {
			//laptop
			screenType.set(3);
		}

		if (window.location !== window.parent.location) {
			// The page is in an iframe
			iframe.set(true);
		}
	});
</script>

<svelte:component this={Geometry} />

<div class="app">
	{#if $screenType}
	<header>
		<Header />
	</header>

	<main>
		<slot />
	</main>

	<!-- <footer>
		<Footer />
	</footer> -->

	{/if}
</div>


<style>
	.app {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
		width: 100%;
		background: var(--background);
	}

	header {
		position: fixed;
		top: 0;
		width: 100%;
		z-index: 10;
	}

	footer {
		position: fixed;
		bottom: 0;
		width: 100%;
		z-index: 10;
	}

	main {
		/* flex: 1;
		display: flex;
		flex-direction: column; */
		/* padding: calc(4 * var(--margin)) calc(4 * var(--margin)); */

		/* margin: 0 auto; */
		width: 100%;
		box-sizing: border-box;
	}

	@media only screen and (max-width: 768px) {
		/* main {
			padding: calc(4 * var(--margin)) calc(3 * var(--margin));
		} */
	}
</style>
