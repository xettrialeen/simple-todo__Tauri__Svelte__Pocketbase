<script>
  import { Router, Link, Route, navigate } from "svelte-routing";

  import "carbon-components-svelte/css/all.css";
  import Registration from "./Routes/auth/registration.svelte";
  let theme = "g90"; // "white" | "g10" | "g80" | "g90" | "g100"

  $: document.documentElement.setAttribute("theme", theme);

  import Home from "./Routes/home.svelte";
  import Login from "./Routes/auth/login.svelte";
  import { Button } from "carbon-components-svelte";
  import pb, { currentUser } from "./lib/db/pocketbaseInstance";
  import Dashboard from "./Routes/dashboard/dashboard.svelte";
  import { onMount } from "svelte";
  // logout Handler

  const logoutHandler = () => {
    try {
      pb.authStore.clear();
      navigate("/", { replace: true });
    } catch (error) {
      console.log(error);
    }
  };

  // if ($currentUser) {
  //   navigate("/", { replace: true });
  // }

  onMount(() => {
    if (!$currentUser) {
      navigate("/", { replace: true });
    } else {
      navigate("/dashboard", { replace: true });
    }
  });
</script>

<main class="container">
  {#if !$currentUser}
    <h1><a href="/">Simple Todo</a></h1>
  {:else if $currentUser}
    <div class="home-logged-in">
      <div class="home-logged-in-wrapper">
        <h1><a href="/">Simple Todo</a></h1>
        <Button on:click={logoutHandler} size="small" kind="danger"
          >Logout</Button
        >
      </div>
    </div>
  {/if}
  <div class="container-inner">
    <Router>
      {#if $currentUser}
        <Route path="/dashboard" component={Dashboard} />
      {:else}
        <Route path="/" component={Home} />
        <Route path="/register" component={Registration} />
        <Route path="/login" component={Login} />
      {/if}
    </Router>
  </div>
</main>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  main {
    padding-left: 20px;
    padding-right: 20px;
    height: 100vh;
  }

  .container-inner {
    display: flex;
    justify-content: center;
  }

  .home-logged-in {
    display: flex;
    flex-direction: row;
    justify-content: center;
    gap: 200px;
    margin-bottom: 24px;
  }

  .home-logged-in-wrapper {
    width: 50%;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }
  .home-logged-in h1 {
    font-size: 25px;
    text-align: right;
  }
</style>
