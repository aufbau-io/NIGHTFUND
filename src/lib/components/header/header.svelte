

<script>
	import Nav from './nav.svelte';
	import { darkMode } from '$lib/store/store';
	// stub data out
	const navItems = [
		{
			name: 'ABOUT',
			href: '/'
		},
		{
			name: 'FUNDING GRID',
			href: '/'
		},
		{
			name: 'APPLY FOR FUNDING',
			href: '/'
		},
	];

	let toggleDarkMode = () => {
		darkMode.set(!$darkMode);
		document.querySelector(':root').classList.toggle('dark-mode');
	};
</script>

<header>
	<main>

		<div class="end">
			<img src="/nf.svg" alt="logo" />
			<p>Revitalising
				club culture</p>
		</div>

		<Nav {navItems} />



	<!-- svelte-ignore a11y-click-events-have-key-events -->
	<!-- <div on:click={() => toggleDarkMode()} class="darkMode">
		<p class:selected={$darkMode == false}>day</p>
		<p class="selected">/</p>
		<p class:selected={$darkMode == true}>night</p>
	</div> -->
</main>
</header>

<style>
	img {
		transform: translateY(-25%);
		height: 200%;
		padding-right: 10px;
	}
	header {
		user-select: none;
		color: var(--primary);
		/* border-bottom: solid 1px var(--primary-50);
		background: var(--background);
		box-shadow: 0px -1px 8px var(--orimary-50); */
	}

	main {
		margin: auto;
		width: 100%;
		display: flex;
		justify-content: space-between;
		align-items: center;
		font-family: var(--font-mono);
		text-transform: uppercase;
		padding: 20px 10px;
		font-size: 14px;
		height: calc(3 * var(--margin));
		font-weight: 500;
	}

	.darkMode {
		display: flex;
	}

	.app_link {
		opacity: 0.3;
	}

	.darkMode p {
		line-height: 110%;
		font-size: 15px;
	
		padding: 10px;
		cursor: pointer;
		opacity: 0.4;
	}

	.end {
		padding-right: 10px;
		display: flex;
		height: 100%;
	}

	p {
		letter-spacing: 0.15em;
	}

	.darkMode p.selected {
		opacity: 1;
	}

	@media only screen and (max-width: 768px) {
		main {
			font-size: 16px;
		}
	}
</style>
