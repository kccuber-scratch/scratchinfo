<script lang="ts">
    import { onMount } from 'svelte';
    let mostFavourited: Array<object> = [{username: "griffpatch"}];
    let problem: boolean = false;
    let loading: boolean = true;
    onMount(() => {
        fetch("/leaderboards/api/favorites").then((res) => {
            if(!res.ok) {
                problem = true;
            } else {
                return res.json();
            }
        }).then((data) => {
            mostFavourited = data;
            loading = false;
        })
    })
</script>
<h1>Most Favourited Leaderboard</h1>
{#if problem == true}
  <div class="alert alert-danger" role="alert">
    Oh no! We can't get this leaderboard. Try again later.
  </div>
{/if}
{#if loading == false}
{#each mostFavourited as user, position}
    <a href="/users/{user["username"]}"><div class="user rounded">#{position + 1}: {user["username"]} <div style="float: right !important; display: inline;">{user["statistics"].favorites} favourites received</div></div></a>
{/each}
{:else}
    Loading...
{/if}
<style>
    .user {
        width: 100%;
        background-color: rgb(218, 218, 112);
        min-height: 20px;
        margin-bottom: 20px;
        padding: 10px;
        color: black;
    }
    a {
        text-decoration: none;
    }
</style>
