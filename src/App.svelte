<script>
	//components
	import Navbar from './Navbar.svelte';
	import ExpensesList from './ExpenseList.svelte'
	import { setContext, onMount, afterUpdate } from 'svelte'
	import Totals from './Totals.svelte'
	import ExpenseForm from './ExpenseForm.svelte';
	import Modal from './Modal.svelte'
	import Github from './Github.svelte'

	//variables
	let expenses = []

	let setName = '';
	let setAmount = null;
	let setId = null;

	let isFormOpen = false;

	$: isEditing = setId?true:false;
	$: total = expenses.reduce((acc, curr)=>{
		return (acc += curr.amount);
	},0);

	function showForm() {
		isFormOpen = true;
	}

	function hideForm() {
		isFormOpen = false;
		setName = '';
		setAmount = null;
		setId = null;
	}

	function removeExpense(id){
		expenses = expenses.filter(item => item.id !== id);
	}

	function setModifiedExpense(id) {
		let expense = expenses.find(item => item.id === id);
	
		setId = expense.id;
		setName = expense.name;
		setAmount = expense.amount;
		showForm();
	}

	function editExpense({name,amount}){
		expenses = expenses.map(item => {
			//Man kan alltså uppdatera direkt i returnen...
			return item.id === setId ? {...item, name, amount}:{...item}
		});
		setId = null;
		setAmount = null;
		setName = "";
	}

	function clearExpenses() {
		expenses = [];
	}

	function addExpense({name, amount}){
		let expense = {id:Math.random()*Date.now(), name, amount};
		expenses = [expense, ...expenses];
		
	}
	
	function setLocalStorage() {
		localStorage.setItem('expenses', JSON.stringify(expenses));
	}

	onMount(() => {
		expenses = localStorage.getItem("expenses") ? JSON.parse(localStorage.getItem("expenses")) : [] ;
	});
	afterUpdate(() => {
		console.log("after update");
		//Nu kommer lovcaltstorage att uppdateras varje gång som ett värde uppdateras. Blir lite renare kod än att ha denna utspridd
		setLocalStorage();
	})

	setContext("remove", removeExpense)
	setContext("modified", setModifiedExpense)
</script>

<Navbar {showForm}/>
<main class="content"> 
	<Github/>
	<!-- {#if isFormOpen}
	<Modal>
		<ExpenseForm {addExpense} name ={setName} amount={setAmount} {isEditing} {editExpense} {hideForm}/>
	</Modal>
	{/if}
	<Totals title="total expenses" {total}/>
	<ExpensesList expenses={expenses}/>
	<button type="button" class="btn btn-block" on:click={clearExpenses}>
		clear expenses
	</button> -->
</main>


