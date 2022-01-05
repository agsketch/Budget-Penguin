<script>
	/* Module Imports */
	import { afterUpdate, onMount } from 'svelte'

	/* Component Imports */
	import Navbar from './Components/Navbar.svelte'
	import Footer from './Components/Footer.svelte'
	import Card from './Components/Card.svelte'
	import Modal from './Components/Modal.svelte'

	/* Hooks */
	let expenses = []

	onMount(() => {
		expenses = localStorage.getItem('expenses') ? JSON.parse(localStorage.getItem('expenses')) : []
	})

	afterUpdate(() => {
		localStorage.setItem('expenses', JSON.stringify(expenses))
	})

	/* Functions */
	function addExpense(e) {
		expenses = [e.detail, ...expenses]
		closeModal()
	}

	function changeExpense(e) {
		expenses = expenses.filter((exp) => exp.id !== e.detail.id)
		expenses = [e.detail, ...expenses]
		closeModal()
	}

	function editExpense(e) {
		addMode = false
		modalData = expenses.filter((exp) => exp.id == e.detail)[0]
		modalVisible = true
	}

	function deleteExpense(e) {
		expenses = expenses.filter((exp) => exp.id !== e.detail)
	}

	function clearExpenses() {
		expenses = []
	}

	/* State */
	$: sumExpenses = expenses.reduce((cur, acc) => {
		return cur + acc.amount
	}, 0)

	/* Modal */
	let modalVisible = false
	let modalData = {}
	let addMode = true
	const closeModal = () => (modalVisible = false)
	const openModal = () => {
		addMode = true
		modalData = {
			id: new Date() * Math.random(),
			title: '',
			amount: 0,
			type: 'Must'
		}
		modalVisible = true
	}
</script>

<!-- Modal -->
{#if modalVisible}
	<Modal
		{modalData}
		{addMode}
		on:addExpense={addExpense}
		on:closeModal={closeModal}
		on:changeExpense={changeExpense}
	/>
{/if}
<!-- Modal -->

<!-- Body -->
<Navbar />

<div class="container">
	{#if expenses.length > 0}
		<div class="pill">Total Monthly Expenses: ${sumExpenses}</div>
		<h1>Expense List</h1>
		{#each expenses as expense (expense.id)}
			<Card on:deleteExpense={deleteExpense} on:editExpense={editExpense} {...expense} />
		{/each}

		<button on:click={clearExpenses} class="btn btn-error">
			<span class="material-icons"> delete </span>
			CLEAR EXPENSES
		</button>
	{:else}
		<div class="pill">Get Started by Adding Monthly Expenses!</div>
	{/if}
</div>

<button on:click={openModal} class="btn btn-primary btn-fab">
	<span class="material-icons bold"> add </span>
	ADD EXPENSE
</button>

<Footer />
<!-- Body -->
