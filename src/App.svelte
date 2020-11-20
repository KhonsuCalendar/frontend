<script>
	import Month from './Month.svelte';
</script>

<main>
    <Month />
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 99%;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>