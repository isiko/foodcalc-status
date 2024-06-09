<script>
  export let meal;

  import { page } from '$app/stores'
  const adminPassword = $page.url.searchParams.get('admin')
  let isAdmin =  adminPassword == "TEST";

  function updateMeal() {
    fetch(`https://essen.campus-kit.de/api/${meal.meal_id}`, {
    		method: 'POST',
        headers: {
          "Content-Type": "application/json",
        },
    		body: JSON.stringify(meal.status)
    	})
  }
</script>
<div class="bg-unifest-green m-5 p-3 rounded-md">
  <p> "{meal.status.recipe}" ({meal.meal_id}) </p>
  {#if !isAdmin}
    <p> Status: 
      {#if meal.status.eta == 0} 
        ✅ Serving 
      {:else if meal.status.eta > 0} 
        <p>⚠️ More is on the way, comming in {meal.status.eta}min </p>
        <p> ETA: {new Date(meal.status.last_modified + meal.status.eta * 60).toISOString()} </p>
      {:else if meal.status.eta < 0} 
        ❌ Sorry, this meal has finished serving :/
      {/if}
    </p>
  {:else}
    <p> ETA: <p> <input type="number" bind:value={meal.status.eta}>
    <p> Custom Message: <p> <input type="text" bind:value={meal.status.msg}>
    <button on:click={updateMeal}> UPDATE! </button>
  {/if}
</div>
