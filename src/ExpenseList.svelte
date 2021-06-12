<script>
    import SectionTitle from './Title.svelte';
    import Expense from './Expense.svelte'
    import {fly} from 'svelte/transition'
    export let expenses = [];
</script>

<style>
    h2 {
        text-transform: capitalize;
    }
</style>
<section>
    <SectionTitle title="expense list"/>
    <ul>
		{#each expenses as expense, index (expense.id)}
        <div transition:fly={{x:200, delay:index*400}}>
			<Expense {expense} />
        </div>
        {:else}
            <h2> no expenses added to  the list</h2>
		{/each}
	</ul>
</section>