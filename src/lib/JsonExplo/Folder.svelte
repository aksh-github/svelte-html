<script>
  import File from "./File.svelte";
  import { slide } from "svelte/transition";
  import { afterUpdate, beforeUpdate, onMount } from "svelte";

  export let expanded = false;
  let currentName = "";
  export let displayName;
  export let parentName;
  export let files;

  function toggle() {
    console.log(currentName, parentName, displayName);
    expanded = !expanded;
  }

  onMount(() => {
    // console.log(parentName, displayName);
    currentName = parentName; //? parentName + "." + displayName : displayName;
    parentName = currentName;
  });

  // afterUpdate(() => {
  //   parentName = parentName ? parentName + "." + displayName : displayName;
  //   console.log(parentName, displayName);
  // });

  function liClicked(ob) {
    console.log(ob);
  }
</script>

<span name={parentName} class:expanded on:click={toggle}
  >{displayName || ""} {files instanceof Array ? " [ ]" : " { }"}</span
>

{#if expanded}
  <ul transition:slide={{ duration: 300 }}>
    {#each Object.entries(files) as file, idx}
      <li
        name={parentName + "." + file[0]}
        on:click={(e) => {
          e.stopPropagation();
          liClicked(file);
        }}
      >
        {#if typeof file[1] === "object"}
          {file[0]}: <svelte:self files={file[1]} parentName={parentName + "." + file[0]} displayName={""} />
        {:else}
          {file[0]}:
          <div>{file[1]}</div>
          <!-- <input type="text" value={file[1]} /> -->
        {/if}
      </li>
    {/each}
  </ul>
{/if}

<style>
  span {
    padding: 0 0 0 1.5em;
    background: url(/folder.svg) 0 0.1em no-repeat;
    background-size: 1em 1em;
    /* font-weight: bold; */
    cursor: pointer;
  }

  .expanded {
    background-image: url(/folder-open.svg);
  }

  ul {
    padding: 0.2em 0 0 0.5em;
    margin: 0 0 0 0.5em;
    list-style: none;
    border-left: 1px solid #eee;
  }

  li {
    padding: 0.2em 0;
  }
</style>
