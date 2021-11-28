<script lang="ts">
  export let id: string;
  export let description: string;
  export let isComplete: boolean;
  export let onToggleTaskStatus: (id: string) => void;

  const toggleCompletion = () => {
    onToggleTaskStatus(id);
  };
</script>

<!-- <button on:click={increment}> -->
<p
  class={`task ${isComplete ? "completed" : "not-completed"}`}
  on:click={toggleCompletion}
>
  {description}
</p>

<style>
  .task {
    border: 1px solid #808080;
    color: #ff3e00;
    cursor: pointer;
    padding: 10px 0;
    width: 50%;
  }
  .task:hover {
    background-color: #f0f0f0;
  }
  .completed {
    color: gray;
    text-decoration: line-through;
  }
</style>
