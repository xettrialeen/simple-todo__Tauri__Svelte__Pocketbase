<script>
  import { Button } from "carbon-components-svelte";
  import { Link } from "svelte-routing";

  
</script>

<div class="home">
  <p>Wlecome to Simple Todo</p>

  <div class="authentication">
    <Link to="/register">
      <Button size="small">Register</Button>
    </Link>
    <Link to="/login">
      <Button size="small" kind="secondary">Login</Button>
    </Link>
  </div>
</div>

<style>
  .authentication {
    margin-top: 20px;
  }
</style>
