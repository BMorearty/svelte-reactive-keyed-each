<script>
  import { invalidateAll } from '$app/navigation';

  const { data } = $props();
  let items = $state(copyItems(data.items));
  $effect(() => {
    items = copyItems(data.items);
  });
  let price = $state(200);

  function changePrice() {
    items[1].price = price;
  }

  // In my app I use lodash's cloneDeep, but for this demo I'll use a simple function
  // to avoid dependencies
  function copyItems(original) {
    let newItems = [];
    for (let i = 0; i < original.length; i++) {
      newItems[i] = { ...original[i] };
    }
    return newItems;
  }
</script>

<h2>Unkeyed <code>#each</code> (reactive)</h2>
{#each items as item}
  <p>{item.name} costs ${item.price}</p>
{/each}

<h2>Keyed <code>#each</code> (should be reactive but is not)</h2>
{#each items as item (item.id)}
  <p>{item.name} costs ${item.price}</p>
{/each}

<h2>Change 2nd item’s price. The keyed <code>#each</code> won’t update:</h2>
<form on:submit={changePrice}>
  <input bind:value={price} />
  <button type="submit">Change price</button>
</form>

<h2>Reload</h2>
<button on:click={() => invalidateAll()}><code>invalidateAll()</code></button>
