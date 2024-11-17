<script>
  let todos = $state([""]); //Array to hold todo items
  $effect(() => {
    let localdb = JSON.parse(localStorage.getItem("todos"));
    if (localdb) {
      todos = localdb;
    }
  });

  let itemInput = $state("");

  const removeItem = (e) => {
    let id = e.target.value;
    todos.splice(id, id);
    localStorage.setItem("todos", JSON.stringify(todos));
  };

  const addItem = () => {
    todos.push(itemInput);
    localStorage.setItem("todos", JSON.stringify(todos));
  };

  const handleDone = (e) => {
    let id = e.target.value;
    todos.splice(id, id);

    let itemToUpdate = JSON.parse(localStorage.getItem("todos"))[id];

    let updatedItem = `${itemToUpdate} @true!`;

    todos.push(updatedItem);

    localStorage.setItem("todos", JSON.stringify(todos));
  };
</script>

<main class="container-fluid">
  <h1>Todo App</h1>
  <div id="inputDiv">
    <input bind:value={itemInput} type="text" class="form-control" />
    <br />
    <div class="text-center">
      <button class="btn btn-success" onclick={addItem}>Add</button>
    </div>
  </div>
  <br />
  <div>
    <h1>Todo List</h1>
    <ol>
      {#each todos as todo, i}
        {#if i > 0}
          {#if todo.includes("@true!")}
            <li class="text-secondary" style="text-decoration: line-through;">
              {todo.replace("@true!", "")}
              <div>
                <button value={i} onclick={removeItem} class="btn btn-danger"
                  >Remove</button
                >
              </div>
            </li>
          {:else}
            <li>
              {todo}

              <div id="btns">
                <button value={i} onclick={handleDone} class="btn btn-info"
                  >Done</button
                >
                <button value={i} onclick={removeItem} class="btn btn-danger"
                  >Remove</button
                >
              </div>
            </li>
          {/if}
          <br />
        {/if}
      {/each}
    </ol>
  </div>
</main>

<style>
  h1 {
    color: teal;
    text-align: center;
  }

  #inputDiv {
    max-width: 30rem;
    margin: auto;
    border: 2px solid tan;
    padding: 12px;
    border-radius: 6px;
  }

  input {
    border: 1px solid tan;
  }

  @media (min-width: 769px) {
    ol {
      margin-left: 40%;
    }
  }
  ol,
  li {
    font-size: larger;
    font-family: Arial, Helvetica, sans-serif;
  }
</style>
