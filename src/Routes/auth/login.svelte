<script>
  import { TextInput, Button,PasswordInput } from "carbon-components-svelte";
  import { registrationData } from "../../lib/stores/store.js";
  import { Link ,navigate } from "svelte-routing";
  import pb from "../../lib/db/pocketbaseInstance.js";

  let formData = {};

  async function loginHandler() {
    try {
      const { username, password } = formData;
      // login req from pb
      const loginResponse = await pb
        .collection("users")
        .authWithPassword(username, password);

      if (pb.authStore.isValid) {
        navigate('/dashboard')
      } else {
        console.log("OOPs Authentication failed");
      }
    } catch (error) {
      console.log(error);
    }
  }
</script>

<div class="registration">
  <h3>User Registration</h3>
  <form>
    <TextInput
      invalidText="User name is already taken. Please try another."
      labelText="User name"
      placeholder="Enter user name..."
      style="margin-bottom:24px"
      bind:value={formData.username}
    />

  

    <PasswordInput required   style="margin-bottom:24px" bind:value={formData.password} labelText="Password" placeholder="Enter password..." />

    <div class="submit">
      <Button on:click={loginHandler} expressive size="field">Login</Button>
    </div>
  </form>

  <span class="link">Not a user? <Link to="/register">Register</Link> </span>
</div>

<style>
  .registration {
    width: 50%;
    display: flex;
    flex-direction: column;
    margin-top: 40px;
    justify-content: flex-start;
  }

  .registration h3 {
    text-align: left;
    font-size: 1.8rem;
    margin-bottom: 24px;
  }

  .submit {
    width: 100%;
    display: flex;
  }

  .link {
    margin-top: 36px;
    text-align: left;
  }
</style>
