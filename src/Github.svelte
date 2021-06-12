<script>
    import {onMount} from 'svelte'
    let users = [];
    let loading = true;

    onMount(async () => {
        let userData = await fetch('https://api.github.com/users')
        let githubusers = await userData.json();
        users = githubusers;
        loading = false;
    });
</script>
<style>
    h2{
        text-align: center;
    }
</style>
{#if loading} 
    <h2>Loading...</h2>
{:else}
    <section>
        {#each users as user}
        <article>
            <img src={user.avatar_url} alt={user.login}>
            <div class="user-info">
                <h2>Whattt</h2>
                <h3>User dupet: {user.login}</h3>
                <a href={user.html_url} class="btn-primary" target="_blank"> githuburl</a>
            </div>
        </article>
        {/each}
    </section>
{/if}