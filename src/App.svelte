<script lang="ts">
  import Router, { link } from "svelte-spa-router";
  import Counter from "./lib/Counter.svelte";
  import Emoji from "./lib/Emoji.svelte";
  import SelectContent from "./lib/SelectContent.svelte";
  import NotFound from "./lib/NotFound.svelte";
  import Recursive from "./lib/Recursive/Recursive.svelte";
  import JsonRecursive from "./lib/JsonExplo/JsonRecursive.svelte";
  import DeskNotify from "./lib/DeskNotify.svelte";

  const routes = {
    // Exact path
    "/": Counter,
    "/emoji": Emoji,
    "/selectcontent": SelectContent,
    "/recursive": Recursive,
    "/jsonrecursive": JsonRecursive,
    "/desknotify": DeskNotify,
    // Catch-all
    // This is optional, but if present it must be the last
    "*": NotFound,
  };
</script>

<header>
  <nav>
    <a href="/" use:link>Home</a>
    <a href="/emoji" use:link>Emoji Example</a>
    <a href="/selectcontent" use:link>Select n Copy</a>
    <a href="/recursive" use:link>Recursive</a>
    <a href="/jsonrecursive" use:link>Json Explorer</a>
    <a href="/desknotify" use:link>Desk Notification</a>
  </nav>
</header>
<main>
  <Router {routes} />
</main>

<style>
  main {
    padding: 5em;
  }

  nav {
    /* max-width: 400px; */
    overflow: auto;
    white-space: nowrap;
  }

  @media (max-width: 425px) {
    nav {
      max-width: 400px;
    }
  }

  a {
    margin: 1em;
    display: inline-block;
  }
</style>
