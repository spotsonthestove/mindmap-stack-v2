<script lang="ts">
  import MindMap from '$lib/MindMap.svelte';
  import BranchAdder from '$lib/BranchAdder.svelte';
</script>

<main>
  <h1>3D Mind Map</h1>
  <div class="mind-map-container">
    <MindMap />
    <BranchAdder />
  </div>
</main>

<style>
  .mind-map-container {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-top: 2rem;
  }
</style>