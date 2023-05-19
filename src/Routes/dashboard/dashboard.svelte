<script>
  import { TextArea, Button, Loading } from "carbon-components-svelte";
  import pb, { currentUser } from "../../lib/db/pocketbaseInstance";
  import { onMount } from "svelte";

  let textData;
  let todos = [];
  onMount(async () => {
    if (pb.authStore.isValid) {
      const response = await pb.collection("todo").getFullList({
        sort: "-created",
      });
      todos = Object.values(response);
    }
  });

  const collectData = async () => {
    try {
      if (pb.authStore.isValid) {
        const record = await pb.collection("todo").create({
          todos: textData,
          user: pb.authStore.model.id,
        });

        const getList = await pb.collection("todo").getFullList({
          sort: "-created",
          expand: "user",
        });
        todos = Object.values(getList);
      }
    } catch (error) {}
  };
</script>

<div class="dashboard">
  <TextArea
    labelText="App description"
    placeholder="Enter a description..."
    maxCount={100}
    bind:value={textData}
  />

  <Button on:click={collectData}>Add Todo</Button>

  <div class="results">
    <h2>Todo Results</h2>

    <ul>
      {#await todos}
        <li><span><Loading withOverlay={false} small /></span></li>
      {:then todos}
        {#each todos as todo}
          <li><span>{todo.todos}</span></li>
        {/each}
      {/await}
    </ul>
  </div>
</div>

<style>
  .dashboard {
    width: 50%;
    display: flex;
    flex-direction: column;
    gap: 20px;
  }
  .results {
    text-align: left;
  }
  .results h2 {
    text-align: left;
    margin-top: 24px;
  }
  .results ul {
    margin-top: 48px;
  }
</style>
