<script>
	let lorem = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed id est urna. Morbi suscipit tristique mollis. Maecenas a felis et nunc ullamcorper volutpat. Sed sollicitudin libero ac dui porta, ac laoreet neque mattis. Aliquam bibendum rutrum urna, ac tempus lorem cursus eget. Curabitur blandit nisi nec libero tincidunt egestas. Nam varius sem id dui porta, dictum eleifend odio aliquet. Donec viverra neque sapien, et pellentesque sem efficitur et. Donec mattis est eget consequat lacinia. Vivamus quis dolor at neque accumsan sollicitudin. Curabitur turpis neque, tincidunt sit amet blandit in, tempus et elit. Duis mattis odio augue, a venenatis ligula aliquam sed. Vivamus luctus massa a convallis auctor."
</script>

<svelte:head>
	<title>NIGHTFUND</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section class="section head">
		<!-- <h1>NIGHT FUND</h1> -->
		<img src="/nf.svg" alt="logo" />
</section>
<section class="section">
		<h2>NightFund </h2>
		<div>
			<h1>THE SPACE</h1>
		<p>{lorem}</p>
	</div>
</section>
<section class="section" />
<section class="section">
	<h2>NightFund </h2>
	<div>
		<h1>THE PROBLEM</h1>
	<p>{lorem}</p>
</div>
</section>
<section class="section" />
<section class="section">
	<h2>NightFund</h2>
	<div>
		<h1>THE SOLUTION</h1>
	<p>{lorem}</p>
</div>
</section>



<style>
	section {
		position: relative;
		display: flex;
		flex-flow: row nowrap;
		align-items: flex-start;
		justify-content: flex-start;
		flex: 1;
		/* background: var(--background); */
		/* border: solid 1px var(--primary); */
		height: 100vh;
		width: 100%;
		max-width: 1200px;
		max-height: 70%;
		height: calc(var(--vh, 1vh) * 100);

		padding: 200px 40px 200px 40px;
		margin: auto;
		gap: 40px;
/* 
		background: #23232340; */
	}



	section.head {
		padding: 120px 40px;
		background: none;
		margin: auto;
	}

	section div {
		display: flex;
		flex-flow: column nowrap;
		align-items: flex-start;
		max-width: 1500px;
		gap: 40px;

	}

	h2 {
		min-width: 280px;
		padding: 20px;
	}

	h1 {
		color: transparent;

		text-align: start;
		font-family: var(--font-header);
		font-weight: 500;
		-webkit-text-stroke: 1px var(--accent);
		font-size: 10vh;
	}

	img {
		width: 40vh;
		margin: auto;
	}

	h2 {
		font-weight: 500;
		font-size: 2rem;
		font-family: var(--font-mono)
	}

	p {
		font-weight: 300;
		font-size: 1.4rem;
	}

section:nth-child(odd) {
  justify-content: flex-end;
}

</style>
