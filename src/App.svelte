<script lang="ts">
  import Task from "./lib/Task/index.svelte";
  import type { ITask } from "./lib/Task/task.type";
  import { v4 as uuidv4 } from "uuid";
  import { onMount } from "svelte";

  let taskInput;
  let newTask = "";
  let tasks = (JSON.parse(localStorage.getItem("tasks")) || []) as ITask[];
  $: completedTasks = tasks.filter((t) => t.complete);
  $: nonCompletedTasks = tasks.filter((t) => !t.complete);

  const toggleTaskStatus = (id: string) => {
    tasks = tasks.map((task: ITask) =>
      task.id === id ? { ...task, complete: !task.complete } : task
    );
    localStorage.setItem("tasks", JSON.stringify(tasks));
  };

  const createNewTask = () => {
    tasks = [
      ...tasks,
      {
        id: uuidv4(),
        description: newTask,
        complete: false,
      } as ITask,
    ];
    newTask = "";
    taskInput.focus();
    localStorage.setItem("tasks", JSON.stringify(tasks));
  };

  onMount(() => {
    taskInput.focus();
  });
</script>

<main>
  <h1>Paper ToDo</h1>

  <div>
    <input bind:this={taskInput} type="text" bind:value={newTask} />
    <button on:click={createNewTask}>Create</button>
  </div>

  <h2>Tasks to complete:</h2>

  {#if nonCompletedTasks.length === 0}
    <p>No tasks to complete. Good job!</p>
  {/if}
  {#each nonCompletedTasks as task}
    <Task
      id={task.id}
      description={task.description}
      isComplete={task.complete}
      onToggleTaskStatus={toggleTaskStatus}
    />
  {/each}

  <h2>Completed tasks:</h2>

  {#if completedTasks.length === 0}
    <p>No tasks completed yet.</p>
  {/if}
  {#each completedTasks as task}
    <Task
      id={task.id}
      description={task.description}
      isComplete={task.complete}
      onToggleTaskStatus={toggleTaskStatus}
    />
  {/each}
</main>

<style>
  :root {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
      Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  }

  main {
    text-align: center;
    padding: 1em;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4rem;
    font-weight: 100;
    line-height: 1.1;
    margin: 2rem auto;
    max-width: 14rem;
  }

  @media (min-width: 480px) {
    h1 {
      max-width: none;
    }
  }
</style>
