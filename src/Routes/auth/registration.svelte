<script>
  import { TextInput, Button } from "carbon-components-svelte";
  import { registrationData } from "../../lib/stores/store.js";
  import { Link , navigate} from "svelte-routing";

  import pb from "../../lib/db/pocketbaseInstance.js";
  let formData = {};
  async function receiveDataFromRegistration() {
    try {
      // registrationData.set(formData);
      const response = await pb.collection("users").create({
        email: formData.email,
        password: formData.password,
        passwordConfirm: formData.confirmPassword,
        name: formData.username,
        username: formData.username
      });

      formData = {};
      console.log(response);

      navigate("/login")
    } catch (error) {}
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
    <TextInput
      invalidText="User name is already taken. Please try another."
      labelText="Your Email"
      placeholder="Enter email...  eg xyz@gmail.com"
      type="email"
      required
      style="margin-bottom:24px"
      bind:value={formData.email}
    />
    <TextInput
 
      invalidText="passwords length must be greater then 10."
      labelText="Your Password"
      placeholder="Enter Password"
      type="password"
      required
      style="margin-bottom:24px"
      bind:value={formData.password}
    />
    <TextInput
      invalidText="Password Didn't Match"
      labelText="Confirm Your Password"
      placeholder="Confirm Password"
      type="password"
      required
      style="margin-bottom:24px"
      bind:value={formData.confirmPassword}
    />

    <div class="submit">
      <Button on:click={receiveDataFromRegistration} expressive size="field"
        >Register</Button
      >
    </div>
  </form>

  <span class="link">already a user? <Link to="/login">Login</Link></span>
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
