<script>
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section class="section">
		<h1>NIGHT FUND</h1>
</section>
<section class="section">
		<h1>THE SPACE</h1>
</section>
<section class="section">
		<h1>THE PROBLEM</h1>
</section>


<style>
	section {
		position: relative;
		display: flex;
		align-items: center;
		justify-content: flex-start;
		flex: 1;
		/* background: var(--background); */
		/* border: solid 1px var(--primary); */
		height: 100vh;
		height: calc(var(--vh, 1vh) * 100);
	}

	h1 {
		width: 100%;
		color: var(--background);
		font-family: var(--font-header);
		font-weight: 500;
		-webkit-text-stroke: 1px var(--accent);
		font-size: 20vh;
	}

section:nth-child(odd) {
  justify-content: flex-end;
}

</style>
