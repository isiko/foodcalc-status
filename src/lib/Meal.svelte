<script>
  export let meal;

  import { page } from '$app/stores'
  const adminPassword = $page.url.searchParams.get('admin')
  let isAdmin =  adminPassword == "TEST";

  function updateMeal() {
    fetch(`http://10.1.1.4:8090/${meal.meal_id}`, {
    		method: 'POST',
        headers: {
          "Content-Type": "application/json",
        },
    		body: JSON.stringify(meal.status)
    	})
  }
</script>
<div class="bg-blue-300 m-5 p-1">
  <p> Meal: {meal.status.recipe} ({meal.meal_id}) </p>
  {#if !isAdmin}
    <p> Status: 
      {#if meal.status.eta == 0} 
        Serving 
      {:else if meal.status.eta > 0} 
        More is on the way, ETA: +{meal.status.eta}min 
      {:else if meal.status.eta < 0} 
        Sorry, this meal has finished serving :/
      {/if}
    </p>
  {:else}
    <p> ETA: <p> <input type="number" bind:value={meal.status.eta}>
    <p> Custom Message: <p> <input type="text" bind:value={meal.status.msg}>
    <button on:click={updateMeal}> UPDATE! </button>
  {/if}
</div>
