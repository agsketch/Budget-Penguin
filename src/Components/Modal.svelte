<script>
	/* Imports */
	import { createEventDispatcher } from 'svelte'
	import {fade} from 'svelte/transition'
	const dispatch = createEventDispatcher()

	/* Data */
	let clear = {
		id: new Date() * Math.random(),
		title: '',
		amount: 0,
		type: 'Must'
	}

	/* Functions */
	const closeModal = () => dispatch('closeModal')
	const changeExpense = () => {
		if (addMode) {
			dispatch('addExpense', modalData)
		} else {
			dispatch('changeExpense', modalData)
		}
	}

	/* Bindings */
	export let modalData = clear
	export let addMode = true
	$: valid = modalData.title.length > 1 && modalData.amount > 0 && modalData.type.length > 1 ? true : false
</script>

<section transition:fade>
	<div class="body">
		<h2>ADD EXPENSE</h2>

		<div class="input-group">
			<label for="title">Expense Title</label>
			<input type="text" bind:value={modalData.title} />
		</div>

		<div class="input-group">
			<label for="amount">Amount</label>
			<input type="number" min="0" bind:value={modalData.amount} />
		</div>

		<div class="input-group">
			<label for="type">Type</label>
			<select name="type" id="type" bind:value={modalData.type}>
				<option value="Must">Must (Food, Water, Shelter)</option>
				<option value="Need">Need (Commodities & Basic Needs)</option>
				<option value="Want">Want (Luxury/Optional Expenses)</option>
			</select>
		</div>

		<div class="btn-group">
			<button on:click={closeModal} class="btn btn-error">
				<span class="material-icons"> close </span> CANCEL</button
			>
			<button disabled={!valid} class="btn btn-primary bold" on:click={changeExpense}>
				<span class="material-icons bold"> send </span>
				SUBMIT</button
			>
		</div>
	</div>
</section>

<style>
	section {
		width: 100vw;
		height: 100vh;

		position: absolute;
		z-index: 99;

		display: grid;
		place-items: center;

		background-color: rgba(0, 0, 0, 0.5);
		backdrop-filter: blur(2px);
	}

	.body {
		background-color: var(--surface-1);
		width: 90%;
		max-width: 500px;
		border-radius: var(--corner-round);
		box-sizing: border-box;
		padding: 20px;
	}

	h2 {
		width: 100%;
		text-align: center;
		font-size: 1.25rem;
		color: var(--on-surface);
		padding-bottom: 10px;
		border-bottom: var(--on-primary-container) 2px solid;
	}

	.input-group {
		width: 100%;
		margin-block: 20px;
		box-sizing: border-box;
	}

	label {
		display: block;
		font-size: 1.1rem;
		color: var(--neutral);
		margin-bottom: 5px;
	}

	input,
	select {
		box-sizing: border-box;
		width: 100%;
		background-color: var(--surface-1);
		border: 1px solid var(--on-primary-container);
		border-radius: var(--corner-round);
		padding: 5px;
		font-size: 1.2rem;
	}

	.btn-group {
		display: flex;
	}
</style>
