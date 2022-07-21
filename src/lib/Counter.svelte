<script lang="ts">
  import { onDestroy, onMount, beforeUpdate, afterUpdate } from "svelte";
  import { store } from "../store";

  const increment = () => {
    // $store.count +=1;
    // store.set({count: $store.count +10, name: $store.name})
    store.update((state) => {
      return { ...state, count: state.count + 10 };
    });
  };

  onMount(() => {
    console.log("compo mounted");
  });

  onDestroy(() => {
    console.log("compo will destroy");
  });

  beforeUpdate(() => {
    console.log("compo b4 update");
  });

  afterUpdate(() => {
    console.log("compo after update");
  });
</script>

<button on:click={increment}>
  count is {$store.count}
</button>
