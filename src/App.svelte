<script>
	// init
	import { onMount } from 'svelte';
	var todos = [];
	var todo = "";

	onMount (() => {
		const existingTodos = localStorage.getItem( 'todos');
		todos = JSON.parse (existingTodos) || [];
	});
	
	// Adds items to the array
	function appendTODO(){
		todos = [...todos, todo];
		todo = "";
		// Stores the array in local storage
		localStorage.setItem('todos',JSON.stringify(todos));
	}
	// Pressing enter will sumbit, the form.
	function enter(){
		if (charCode === 13){
			appendTODO();
		}
	}
	// Removes items from the list
	function gone(i){
		todos.splice(i,1);
		todos = todos;
	}
	// If the input field is not active, pressing delete will delete the first item on the list.
	document.addEventListener("keypress",(e) => {
		if(e.charCode === 8){
			gone(0);
		}
	});
</script>

<main>
	<h1>Tasks</h1>

	<form on:submit|preventDefault="{appendTODO}">
		<input type="text" placeholder="Add tasks..." bind:value="{todo}">
		<input type="submit" value="sumbit" on:keypress="{enter}">
	</form>
	<!--Loops around the array and lists items to the DOM-->
	{#each todos as x, i}
	<li>
		<input type="checkbox">
		<p>{x}</p>
		<p btn on:click="{() => gone(i)}">X</p>
	</li>
	{/each}


</main>

<style>
	p{
		display:inline;
	}
	p,li{
		color: #0004ff;
	}
	input {
		border-radius: 10px;
		background-color: #0004ff;
		border-color: #0004ffaa;
		color: #fff;
	}
	p[btn]{
		color: #df1f06;	
	}
	p[btn]:hover{
		color: #df1f06c8;
	}
	main {
		text-align: left;
		padding: 1em;
		max-width: none;
		margin: 0 auto;
	}	
	h1 {
		color: #0004ff;
		font-family: 'Times New Roman', Times, serif;
		text-transform: uppercase;
		font-size: 3em;
		font-weight: 100;
	}
</style>