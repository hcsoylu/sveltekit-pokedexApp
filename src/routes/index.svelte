<script context="module">
	export async function load({ page }) {
		const url = 'https://pokeapi.co/api/v2/pokemon?limit=150';
		const res = await fetch(url);
		const data = await res.json();
		const loadedPokemon = data.results.map((data, index) => {
			return {
				name: data.name,
				id: index + 1,
				image: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${
					index + 1
				}.png`
			};
		});
		return {
			props: { pokemon: loadedPokemon }
		};
	}
</script>

<script>
	import PokeCard from '../components/pokeCard.svelte';
	export let pokemon;

	let searchTerm = '';
	let filteredPokemon = [];

	$: {
		if (searchTerm) {
			filteredPokemon = pokemon.filter((poke) =>
				poke.name.toLowerCase().includes(searchTerm.toLowerCase())
			);
		} else {
			filteredPokemon = [...pokemon];
		}
	}
</script>

<svelte:head>
	<title>Svelte Kit Pokedex</title>
</svelte:head>

<h1 class="text-4xl text-center my-8 uppercase">Svelte Kit Pokedex</h1>
<input
	bind:value={searchTerm}
	type="text"
	placeholder="Search pokemon"
	class="w-full rounded-md text-lg p-4 border-gray-200 border-2"
/>
<div class="py-4 grid gap-4  md:grid-cols-2 grid-cols-1">
	{#each filteredPokemon as poke}
		<PokeCard {poke} />
	{/each}
</div>
