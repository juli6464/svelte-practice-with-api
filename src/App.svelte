<script>
	// import { onMount } from 'svelte';
	import {fly} from 'svelte/transition';
	import MovieItem from './Movie/Item.svelte';



	// let characters = []; 
	// function fetchCharacters () {
		
	// 	   const URL = 'https://rickandmortyapi.com/api/character'
	// 	   fetch(URL)
	// 		.then(res => res.json())
	// 		.then(({results}) => {
	// 			characters = results

	// 			console.log(characters)
	// 		})
	// } 

	const characters = (async () => {
		const URL = 'https://rickandmortyapi.com/api/character'
		const response= await fetch(URL)
		
		return await response.json()
	})()

	let likedCharacters = [];

	function toggleLike (event) {
		//console.log(event)
		const character = event.detail;

		let index = likedCharacters.findIndex(c => c.id === character.id)

		if (index >=0) {
			likedCharacters.splice(index, 1);
			console.log(likedCharacters);

		} else{
			likedCharacters.push(character);
		}
		likedCharacters= likedCharacters
	}

	$: like = (id) => {
		let index = likedCharacters.findIndex(c => c.id === id)

		return index >= 0
	}

	// onMount(() => {
	// 	console.log('the component has mounted');
	// 	fetchCharacters()
	// });
</script>

<svelte:head>
	<title>Rick and morty Characters  with Svelte</title>
	<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous">
</svelte:head>

<main class="container">
	<div class="row">
		<div class="col-12 col-md-6 col-lg-8 border panel">
			<h2>Best characters</h2>
			<div class="row">
				{#await characters}
				<div class="col-12">
					<p>...loading characters</p>
				</div>

				{:then data}
					{@debug data}
						{#each data.results as character}
							<div  class="col-12 col-md-6 col-lg-4 p-2">
								<MovieItem like={like(character.id)} id={character.id} name={character.name} status={character.status} image={character.image} on:onToggleLike={toggleLike} />
							</div> 
						{/each}
				{/await}
			</div>
			
		</div>
		<div class="col-12 col-md-6 col-lg-4 border panel">
			<h2>Favourite characters</h2>
				<div class="row">
					{#if likedCharacters.length}
						{#each likedCharacters as character, i (character.id)}
							<div in:fly="{{duration:2000, y: 20}}" out:fly="{{duration:800, y: -20}}" class="col-12 col-md-6 col-lg-4 p-2">
								<MovieItem like={like(character.id)} id={character.id} name={character.name} status={character.status} image={character.image} on:onToggleLike={toggleLike} />
							</div> 
						{/each}
					{:else}
							<div class="col-12">
								<p>You don't have favorite characters</p>
							</div>
					{/if}

				</div>
		</div>
	</div>
</main>

<style>
	.panel {
		height: 100vh;
		overflow: auto;
	}
	main {
		text-align: center;
		padding: 0;
		max-width: 240px;
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