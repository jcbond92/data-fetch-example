<script lang="ts">
	import { onMount } from 'svelte';
	import Card from '$lib/card.svelte';
	import Loading from '$lib/loading.svelte';

	let person = 'Skywalker';
	let personSearchTitle: string;
	let results: [];
	let loading = true;
	let resultSingularPlural: string;
	let disabled = false;
	let errorStatus = false;

	const starWarsRequest = () => {
		personSearchTitle = person;
		loading = true;
		disabled = true;

		fetch(`https://swapi.dev/api/people/?search=${person}`)
			.then((response) => response.json())
			.then((data) => {
				results = data.results;
				loading = false;
				disabled = false;
				if (results.length > 1) resultSingularPlural = 'results';
				if (results.length <= 1) resultSingularPlural = 'result';
			})
			.catch((error) => {
				console.error('Error:', error);
				loading = false;
				disabled = false;
				errorStatus = true;
			});
	};

	const handleKeydown = (event) => {
		if (event.keyCode === 13) starWarsRequest();
	};

	onMount(() => {
		starWarsRequest();
	});
</script>

<h1>Star Wars character search</h1>

<section class="force--search">
	<div class="force--search-input-wrapper">
		<input bind:value={person} on:keypress={handleKeydown} />
		<button on:click={starWarsRequest} {disabled}>Search</button>
	</div>
</section>

<section class="force--results">
	{#if loading}
		<div class="force--results-loading">
			<span>searching for "{personSearchTitle}"...</span>
			<Loading />
		</div>
	{/if}
	{#if !loading && !errorStatus}
		<div class="force--results-summary">
			<p>{results.length} {resultSingularPlural} for "{personSearchTitle}"</p>
		</div>
		<div class="force--results-cards">
			{#each results as result}
				<Card cardConfig={result} />
			{/each}
		</div>
	{/if}
	{#if !loading && errorStatus}
		<div class="force--results-summary">
			<p>There is an issue with the API, please try searching again.</p>
			<p>If you have seen this error multiple times, please try again later.</p>
		</div>
	{/if}
</section>

<style>
	h1 {
		font-family: 'Star Jedi', sans-serif;
		font-size: 3rem;
		text-align: center;
		color: var(--white);
	}

	input {
		height: 100%;
		background: none;
		border: none;
		width: 100%;
		font-size: 1rem;
		padding-left: 10px;
		color: var(--pink);
		font-family: 'Nunito Sans', sans-serif;
	}

	.force--search-input-wrapper {
		border: solid var(--pink) 1px;
		height: 2.5em;
		width: 40%;
		display: flex;
		border-radius: 3px;
	}

	.force--results-loading {
		text-align: center;
		color: var(--pink);
	}

	button {
		background: none;
		border: none;
		border-left: solid var(--pink) 1px;
		color: var(--pink);
		font-size: 1rem;
		font-family: 'Nunito Sans', sans-serif;
		cursor: pointer;
		opacity: 1;
		transition: opacity 0.35;
	}

	button:hover {
		background: var(--pink);
		color: var(--dark-blue);
		transition: background 0.35s, color 0.35s;
	}

	button:disabled {
		opacity: 0;
		width: 0;
		height: 0;
		transition: opacity 0.35;
	}

	.force--results {
		margin-top: 2em;
	}

	.force--search {
		display: flex;
		flex-direction: row;
		justify-content: center;
	}

	.force--results-cards {
		display: flex;
		flex-direction: row;
		justify-content: center;
	}

	.force--results-summary {
		text-align: center;
		color: var(--pink);
	}

	@media only screen and (max-width: 768px) {
		.force--results-cards {
			flex-direction: column;
			align-items: center;
		}

		.force--search-input-wrapper {
			width: 80%;
		}
	}
</style>
