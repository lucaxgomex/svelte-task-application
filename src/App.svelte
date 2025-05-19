<script lang="ts">
  import TasksForm from "./components/tasks-form.svelte";
  import TasksList from "./components/tasks-list.svelte";
  import type { Filter, Task } from "./types";
  
  let message = "Tasks App";
  let currentFilter = $state<Filter>("all")
  let tasks = $state<Task[]>([]);
  let totalDone = $derived(
    tasks.reduce(
      (total, task) => total + Number(task.done),
      0
    )
  ); 

  let filteredTaks = $derived.by(() => {
    switch(currentFilter) {
      case "all": {
        return tasks;
      }
      case "done": {
        return tasks.filter(
          (tasks) => tasks.done
        );
      }
      case "todo": {
        return tasks.filter(
          (tasks) => !tasks.done
        );
      }
    }

    return tasks;
  });

  function addTask(newTask: string) {
    tasks.push({
      id: crypto.randomUUID(),
      title: newTask,
      done: false
    });
  }

  function toggleDone(task: Task) {
    task.done = !task.done;
  }

  function removeTask(id: string) {
    const index = tasks.findIndex(
      (tasks) => tasks.id === id
    );

    tasks.splice(index, 1);
  }
</script>

<main>
  <h1>
    {message}
  </h1>
  <TasksForm {addTask} />
  <p> 
    {#if tasks.length}
      {totalDone} / {tasks.length} tasks completed.
    {:else}
      Add a task to get started!
    {/if}
  </p>
  {#if tasks.length}
    <div class="buttom-container">
      <button onclick={() => currentFilter = "all"} class:contrast={currentFilter === "all"} class="secondary">All</button>
      <button onclick={() => currentFilter = "todo"} class:contrast={currentFilter === "todo"} class="secondary">ToDo</button>
      <button onclick={() => currentFilter = "done"} class:contrast={currentFilter === "done"} class="secondary">Done</button>
    </div>
  {/if}
  <TasksList 
    tasks={filteredTaks}
    {toggleDone} 
    {removeTask} 
  />
</main>

<style>
  main {
    margin: 1rem auto;
    max-width: 800px;
  }

  .buttom-container {
    display: flex;
    justify-content: end;
    margin-bottom: 1rem;
    gap: 0.5;
  }
</style>