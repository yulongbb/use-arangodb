<!-- https://eugenkiss.github.io/7guis/tasks#crud -->
<script>
	import { onMount } from "svelte";

	let people = [];

	let prefix = "";
	let _key = "";
	let name = "";
	let i = 0;
	let items = [];

	async function init() {
		people = await fetch(
			`http://localhost:8529/_db/_system/mps/verts`
		).then((r) => r.json());
		console.log(people);
	}

	$: filteredPeople = prefix
		? people.filter((person) => {
				console.log(items);
				const name = `${person._key}`;
				return name.toLowerCase().startsWith(prefix.toLowerCase());
		  })
		: people;

	$: selected = filteredPeople[i];

	$: reset_inputs(selected);

	async function create() {
		people = people.concat({ _key });
		i = people.length - 1;

		const res = await fetch("http://localhost:8529/_db/_system/mps/verts", {
			method: "POST",
			body: JSON.stringify({
				_key,
			}),
		});
		_key = "";

		const json = await res.json();
		console.log(json);
	}

	async function update() {
		const res = await fetch(
			`http://localhost:8529/_db/_system/mps/verts/${selected._key}`,
			{
				method: "PATCH",
				body: JSON.stringify({
					_key,
					name,
				}),
			}
		);
		selected._key = _key;
		selected.name = name;
		people = people;
	}

	async function remove() {
		// Remove selected person from the source array (people), not the filtered array
		const index = people.indexOf(selected);
		people = [...people.slice(0, index), ...people.slice(index + 1)];
		const res = await fetch(
			`http://localhost:8529/_db/_system/mps/verts/${selected._key}`,
			{
				method: "DELETE",
			}
		);
		_key = "";
		i = Math.min(i, filteredPeople.length - 2);
	}

	function reset_inputs(person) {
		_key = person ? person._key : "";
		name = person ? person.name : "";
	}

	onMount(init);
</script>

<input placeholder="filter prefix" bind:value={prefix} />

<select bind:value={i} size={5}>
	{#each filteredPeople as person, i}
		<option value={i}>{person._key}</option>
	{/each}
</select>

<label><input bind:value={_key} placeholder="_key" /></label>
<label><input bind:value={name} placeholder="name" /></label>

<div class="buttons">
	<button on:click={create} disabled={!_key}>create</button>
	<button on:click={update} disabled={!_key || !selected}>update</button>
	<button on:click={remove} disabled={!selected}>delete</button>
</div>

<style>
	* {
		font-family: inherit;
		font-size: inherit;
	}

	input {
		display: block;
		margin: 0 0 0.5em 0;
	}

	select {
		float: left;
		margin: 0 1em 1em 0;
		width: 14em;
	}

	.buttons {
		clear: both;
	}
</style>
