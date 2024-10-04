<script lang="ts">
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  function addBranch() {
    dispatch('addBranch');
  }
</script>

<div class="branch-adder">
  <button on:click={addBranch}>Add Branch</button>
</div>

<style>
  .branch-adder {
    margin-top: 1rem;
  }
  button {
    padding: 0.5rem 1rem;
    background-color: #4CAF50;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 1rem;
  }
  button:hover {
    background-color: #45a049;
  }
</style>