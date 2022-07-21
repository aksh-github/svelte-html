<script lang="ts">
  import Router, { link } from "svelte-spa-router";
  import active from "svelte-spa-router/active";
  import wrap from "svelte-spa-router/wrap";
  import Counter from "./lib/Counter.svelte";

  const routes = {
    // Exact path
    "/": Counter,
    "/emoji": wrap({
      asyncComponent: () => import("./lib/Emoji.svelte"),
    }),
    "/selectcontent": wrap({
      asyncComponent: () => import("./lib/SelectContent.svelte"),
    }),
    "/recursive": wrap({
      asyncComponent: () => import("./lib/Recursive/Recursive.svelte"),
    }),
    "/jsonrecursive": wrap({
      asyncComponent: () => import("./lib/JsonExplo/JsonRecursive.svelte"),
    }),
    "/desknotify": wrap({
      asyncComponent: () => import("./lib/DeskNotify.svelte"),
    }),
    // Catch-all
    // This is optional, but if present it must be the last
    "*": wrap({
      asyncComponent: () => import("./lib/NotFound.svelte"),
    }),
  };
</script>

<header>
  <nav>
    <a href="/" use:link use:active>Home</a>
    <a href="/emoji" use:link use:active>Emoji Example</a>
    <a href="/selectcontent" use:link use:active>Select n Copy</a>
    <a href="/recursive" use:link use:active>Recursive</a>
    <a href="/jsonrecursive" use:link use:active>Json Explorer</a>
    <a href="/desknotify" use:link use:active>Desk Notification</a>
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

  :global(a.active) {
    color: red;
  }
</style>
