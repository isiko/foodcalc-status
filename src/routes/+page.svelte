<script>
    import { page } from '$app/stores'
    import Meal from '$lib/Meal.svelte'
    const adminPassword = $page.url.searchParams.get('admin')
    let isAdmin =  adminPassword == "TEST";

    let status = { }
</script>

{#if isAdmin} ADMIN MODE {/if}

{#await fetch('http://10.1.1.4:8090').then((x) => x.json())}
  Loading...
  (If you see this for more than a second, there is probably something wrong :0)
{:then meals}
  <div class="grid grid-flow-row grid-cols-4">
    {#each meals as meal}
      <Meal meal={meal}/>
    {/each}
  </div>
{:catch error}
   Sorry, there was an error :(
   <script> console.log(error) </script>
{/await}

