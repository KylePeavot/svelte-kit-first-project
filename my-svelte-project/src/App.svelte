<script lang="ts">
	import TodoList from "./TodoList.svelte";

	let name = 'World';

	let data = {
		key: 'value',
		prop1: 'value',
		prop2: 1
	}

</script>

<main>
	<h1>Hello {name}!</h1>
	<TodoList todoAddButtonText="Add Item" />
</main>

<style>
	main {
		padding: 1em;
		max-width: 50%;
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