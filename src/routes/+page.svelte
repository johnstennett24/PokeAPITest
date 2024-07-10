<script lang="ts">
	import { onMount } from 'svelte';
	import Pokemon from './pokemon/+page.svelte';
	import { Router, Route } from 'svelte-routing';
	import {
		Button,
		Theme,
		TextInput,
		Header,
		HeaderNav,
		HeaderNavItem,
		Grid,
		Row,
		Column
	} from 'carbon-components-svelte';
	import 'carbon-components-svelte/css/all.css';

	let forms: [];
	let moves: [];
	let abilities: [];
	let inputValue: string;
	export let stats: [] = [];
	let types: [] = [];
	let id: number;

	let theme = 'g100';

	async function getPokemonData(input: string): Promise<any> {
		const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${input}`);
		return response;
	}

	async function handleClick() {
		const resultFront = document.querySelector('.resultFront');
		const resultBack = document.querySelector('.resultBack');
		const result = document.querySelector('.result');
		const type = document.querySelector('.types');
		const form = document.querySelector('.forms');
		const move = document.querySelector('.moves');
		const ability = document.querySelector('.abilities');
		const number = document.querySelector('.number');

		const response = await getPokemonData(inputValue!.toLowerCase());
		const data = await response.json();
		forms = data.forms;
		moves = data.moves;
		abilities = data.abilities;
		stats = data.stats;
		types = data.types;
		id = data.id;

		if (id > 0) {
			number!.innerHTML = `<h3>No. ${id}</h3>`;
		}
		type!.innerHTML = `
      <h3>Type:</h3>
      <p>${types.map((type) => `${type.type.name}`).join(', ')}</p>
    `;
		resultFront!.innerHTML = `<img src="${data.sprites.front_default}" alt="${data.name}" />`;
		resultBack!.innerHTML = `<img src="${data.sprites.back_default}" alt="${data.name}" />`;
		result!.innerHTML = `<h2>${data.name.toUpperCase()}</h2>`;
		form!.innerHTML = `
      <h3>Forms:</h3>
      <p>${forms.map((form) => `${form.name}`).join(', ')}</p>
      `;
		move!.innerHTML = `
      <h3>Moves:</h3>
      <p>${moves.map((move) => `${move.move.name}`).join(', ')}</p>
    `;
		ability!.innerHTML = `
      <h3>Abilities:</h3>
      <p>${abilities.map((ability) => `${ability.ability.name}`).join(', ')}</p>
    `;
	}

	onMount(() => {
		handleClick();
	});
</script>

<Theme bind:theme />
<Router>
	<Route path="/pokemon" component={Pokemon} />
</Router>

<div class="header-container">
	<Header company="PokeFinder" platformName="Search">
		<HeaderNav>
			<HeaderNavItem href="/pokemon" text="Pokemon"></HeaderNavItem>
			<HeaderNavItem text="Search"></HeaderNavItem>
		</HeaderNav>
	</Header>
</div>

<div class="all">
	<h1>Search</h1>
	<div class="form-grid">
		<div class="text-input-container">
			<TextInput placeholder="Pokemon Name" bind:value={inputValue} />
		</div>
		<div class="button-container">
			<Button kind="primary" on:click={handleClick}>Enter</Button>
		</div>
	</div>
	<div class="result"></div>
	<div class="number"></div>
	<div class="image-grid">
		<div class="resultFront"></div>
		<div class="resultBack"></div>
	</div>
	<div class="stats">
		{#if stats}
			<Grid>
				{#each stats as stat}
					<Row>
						<Column>{stat.stat.name}</Column>
						<Column>{stat.base_stat}</Column>
					</Row>
				{/each}
			</Grid>
		{/if}
	</div>

	<div class="types"></div>
	<div class="forms"></div>
	<div class="moves"></div>
	<div class="abilities"></div>
</div>

<footer>
	<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>
</footer>

<style>
	.stats {
		font-size: 25px;
	}
	.header-container {
		padding: 10px;
	}
	.image-grid {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 0;
		max-width: 100%;
	}
	.image-grid div {
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.text-input-container {
		justify-content: center;
		align-self: center;
		padding: 10px;
		margin: 20px;
	}
	.button-container {
		display: flex;
		justify-content: center;
		padding: 10px;
	}
	h1 {
		text-align: left;
		padding-top: 14px;
		padding-left: 15px;
		margin: 20px;
	}
	.result {
		text-align: center;
	}
	.resultFront {
		text-align: center;
	}
	.resultBack {
		text-align: center;
	}
	footer {
		text-align: center;
	}
	.forms {
		text-align: center;
		text-transform: capitalize;
		padding: 10px;
		margin: 20px;
	}
	.moves {
		text-align: center;
		padding: 10px;
		text-transform: capitalize;
		margin: 20px;
	}
	.abilities {
		text-align: center;
		padding: 10px;
		text-transform: capitalize;
		margin: 20px;
	}
	.stats {
		text-align: center;
		padding: 10px;
		text-transform: capitalize;
		margin: 20px;
	}
	.types {
		text-align: center;
		padding: 10px;
		text-transform: capitalize;
		margin: 20px;
	}
	.number {
		text-align: center;
		padding: 10px;
		text-transform: capitalize;
		margin: 20px;
	}
</style>
