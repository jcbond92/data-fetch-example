<script lang="ts">
	export let cardConfig: {
		name: string;
		birth_year: string;
		hair_color: string;
		skin_color: string;
		gender: string;
		height: string;
		mass: string;
	};
</script>

<div>
	<h2>{cardConfig.name}</h2>
	<ul>
		<li>Birth date: {cardConfig.birth_year}</li>
		<li>Hair color: {cardConfig.hair_color}</li>
		<li>Skin color: {cardConfig.skin_color}</li>
		<li>Gender: {cardConfig.gender}</li>
		<li>Height: {cardConfig.height} cm</li>
		<li>Mass: {cardConfig.mass} kg</li>
	</ul>
</div>

<style>
	h2 {
		font-family: 'Roboto Mono', monospace;
	}

	div {
		border: var(--pink) 1px solid;
		color: var(--pink);
		background-color: none;
		border-radius: 5px;
		margin: 1em;
		padding: 1em 2.5em;
		width: 32%;
		transition: color 0.35s, background-color 0.35s;
	}

	div:hover {
		border: var(--pink) 1px solid;
		background-color: var(--pink);
		color: var(--dark-blue);
		transition: color 0.2s, background-color 0.35s;
	}

	@media only screen and (max-width: 768px) {
		div {
			width: 50%;
		}
	}
</style>
