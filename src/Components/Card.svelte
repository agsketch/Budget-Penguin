<script>
	/* Imports */
	import { createEventDispatcher } from 'svelte'

	const dispatch = createEventDispatcher()

	/* Props */
	export let title, amount, type, id

	/* Functions */
	const deleteExpense = () => dispatch('deleteExpense', id)
	const editExpense = () => dispatch('editExpense', id)

	/* State */
	$: viewAmount = false
</script>

<div class="card {type}">
	<div class="line-1">
		<span>{title}</span>

		<span on:click={() => (viewAmount = !viewAmount)} class="material-icons neutral {viewAmount ? 'expanded' : ''}">
			expand_more
		</span>

		<div class="end">
			<span on:click={editExpense} class="material-icons yellow"> edit </span>
			<span on:click={deleteExpense} class="material-icons red"> delete </span>
		</div>
	</div>
	<div class="line-2 {viewAmount}">
		<span>Amount: ${amount}</span>
		<span>Type: {type}</span>
	</div>
</div>

<style>
	.card {
		width: 100%;
		border: 1px solid var(--outline);
		padding: 10px;
		border-radius: var(--corner-round);
		font-size: 1.25rem;
		box-sizing: border-box;
		margin-block: 10px;
	}

	.line-1 {
		display: flex;
		align-items: center;
	}

	.line-2 {
		margin-block: 5px;
	}

	.false {
		display: none;
	}

	.expanded {
		transform: rotate(180deg);
	}

	.line-2 > span {
		display: block;
		font-size: 1.1rem;
		color: var(--outline);
	}

	.material-icons {
		margin-left: 10px;
		padding: 2px;
		cursor: pointer;
	}

	.end {
		margin-left: auto;
	}

	.Must {
		border-left: 5px solid var(--green);
	}
	.Need {
		border-left: 5px solid var(--yellow);
	}
	.Want {
		border-left: 5px solid var(--red);
	}
</style>
