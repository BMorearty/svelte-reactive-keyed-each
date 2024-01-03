<script>
  import { cloneDeep } from 'lodash-es';
  import { invalidateAll } from '$app/navigation';

  let price = $state(200);
  const { data } = $props();
  let thing = $state({ items: cloneDeep(data.items) });
  $effect(() => {
    thing = { items: cloneDeep(data.items) };
  });

  function changePrice() {
    thing.items[1].price = price;
  }
</script>

<h2>Unkeyed <code>#each</code> (reactive)</h2>
{#each thing.items as item}
  <p>{item.name} costs ${item.price}</p>
{/each}

<h2>Keyed <code>#each</code> (not reactive)</h2>
{#each thing.items as item (item.id)}
  <p>{item.name} costs ${item.price}</p>
{/each}

<h2>Change item 2's price. The keyed <code>#each</code> won’t update:</h2>
<form on:submit={changePrice}>
  <input bind:value={price} />
  <button type="submit">Change price</button>
</form>

<h2>Reload</h2>
<button on:click={() => invalidateAll()}><code>invalidateAll()</code></button>
