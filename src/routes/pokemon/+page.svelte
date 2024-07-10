<script lang="ts">
	import { onMount } from 'svelte';
	import Search from './+page.svelte';
	import { Router, Route } from 'svelte-routing';
	import { Header, Theme, HeaderNav, HeaderNavItem } from 'carbon-components-svelte';
	import 'carbon-components-svelte/css/all.css';

	let theme = 'g100';
</script>

<Theme bind:theme />
<Router>
	<Route path="/routes" component={Search} />
</Router>

<div class="header-container">
	<Header company="PokeFinder" platformName="Search">
		<HeaderNav>
			<HeaderNavItem href="/pokemon" text="Pokemon"></HeaderNavItem>
			<HeaderNavItem href="/" text="Search"></HeaderNavItem>
		</HeaderNav>
	</Header>
</div>

<h1>Pokemon!</h1>

<style>
	h1 {
		padding-top: 50px;
	}
</style>
