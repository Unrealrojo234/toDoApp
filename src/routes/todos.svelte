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
    todos.splice(id, 1);
    localStorage.setItem("todos", JSON.stringify(todos));
  };

  const addItem = (e) => {
    e.preventDefault();
    todos.push(itemInput);
    localStorage.setItem("todos", JSON.stringify(todos));
    itemInput = "";
  };

  const handleDone = (e) => {
    let id = e.target.value;
    let itemToUpdate = JSON.parse(localStorage.getItem("todos"))[id];
    todos.splice(id, 1);

    let updatedItem = `${itemToUpdate} @true!`;

    todos.push(updatedItem);

    localStorage.setItem("todos", JSON.stringify(todos));
  };
</script>

<main class="container-fluid">
  <h1>Todo App</h1>
  <div id="inputDiv">
    <form action="submit" onsubmit={addItem}>
      <input required bind:value={itemInput} type="text" class="form-control" />
      <br />
      <div class="text-center d-grid">
        <button type="submit" class="btn btn-success"
          >Add <i class="fa-solid fa-circle-plus"></i></button
        >
      </div>
    </form>
  </div>
  <br />
  <div>
    {#if todos.length > 1}
      <h1>Todo List</h1>
    {/if}
    <ol>
      {#each todos as todo, i}
        {#if i > 0}
          {#if todo.includes("@true!")}
            <li class="text-secondary" style="text-decoration: line-through;">
              {todo.replace("@true!", "")}
              <div>
                <button value={i} onclick={removeItem} class="btn btn-danger"
                  >Remove <i class="fa-solid fa-trash-can"></i></button
                >
              </div>
            </li>
          {:else}
            <li>
              {todo}

              <div id="btns">
                <button value={i} onclick={handleDone} class="btn btn-info"
                  >Done <i class="fa-solid fa-check"></i></button
                >
                <button value={i} onclick={removeItem} class="btn btn-danger"
                  >Remove <i class="fa-solid fa-trash-can"></i></button
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
