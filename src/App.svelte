<script>
  let item = ""
  let toDoList = []
  let percent = 0
  function addToDo() {
    if (item.task !== "" && item !== "") {
      toDoList = [
        ...toDoList,
        {
          task: item,
          completed: false,
        },
      ]
      item = ""
    }
    setPercent(toDoList)
  }

  function removeItem(key) {
    toDoList.splice(key, 1)
    setPercent(toDoList)
    toDoList = toDoList
  }
  $: toDoList

  function deleteAllItems() {
    toDoList = []
  }
  function setPercent() {
    let done = 0
    for (item of toDoList) {
      if (item.completed) done += 1
    }
    let score = toDoList.length
    return (done / score) * 100 + "%"
  }
  $: percent = setPercent(toDoList)
</script>

<main class="container">
  <div>
    <form on:submit|preventDefault={addToDo}>
      <input
        placeholder="Create task"
        bind:value={item}
        maxlength="20"
        required
      />
      <button>+Add</button>
      <button
        class="deleteAllButton"
        on:click={deleteAllItems}
      >
        Delete All
      </button>
    </form>
    {#each toDoList as item, key}
      <div class="itemContainer">
        <span class={item.completed ? "completed" : ""}>
          {item.task}
        </span>
        <div class="buttonsContainer">
          <input
            type="checkbox"
            checked={item.completed}
            on:click={() =>
              (item.completed = !item.completed)}
          />
          <button
            class="deleteButton"
            on:click={() => removeItem(key)}
          >
            Delete
          </button>
        </div>
      </div>
    {/each}
  </div>
  {#if percent !== "NaN%"}
    <div class="progressBarContainer">
      Выполнено: {percent}
      <div class="progressBarBorder">
        <div class="progressBar" style="width: {percent}" />
      </div>
    </div>
  {/if}
</main>

<style>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 100%;
    justify-content: space-between;
    width: 90%;
  }
  .completed {
    color: #94d8e4;
  }
  .container button:not(.deleteButton, .deleteAllButton) {
    background: #94d8e4;
    color: #ffffff;
    border-radius: 5px;
  }
  .container input {
    border-radius: 5px;
  }
  .itemContainer {
    display: flex;
    border: 1px solid rgba(33, 33, 33, 0.2);
    border-radius: 5px;
    padding: 10px;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
    margin-top: 10px;
    width: 100%;
  }
  .itemContainer:hover {
    box-shadow: 0 0 5px rgba(33, 33, 33, 0.2);
  }
  .buttonsContainer {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-around;
  }
  .deleteButton {
    border: none;
    background-color: transparent;
    color: #c75050;
  }
  .deleteAllButton {
    background-color: #c75050;
    color: #ffffff;
    border-radius: 5px;
  }
  .progressBar {
    margin-bottom: 30px;
    background-color: #94d8e4;
    height: 10px;
  }
  .progressBarBorder {
    border: 1px solid rgba(33, 33, 33, 0.2);
    margin-bottom: 30px;
    height: 10px;
  }
  .progressBarContainer {
    width: 80%;
  }
</style>
