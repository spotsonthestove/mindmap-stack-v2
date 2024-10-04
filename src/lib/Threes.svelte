<script lang="ts">
  import { onMount } from 'svelte';
  import { writable } from 'svelte/store';

  interface Branch {
    id: number;
    text: string;
    x1: number;
    y1: number;
    x2: number;
    y2: number;
  }

  const branches = writable<Branch[]>([
    { id: 1, text: '', x1: 250, y1: 250, x2: 150, y2: 150 },
    { id: 2, text: '', x1: 250, y1: 250, x2: 350, y2: 150 }
  ]);

  let canvas: HTMLCanvasElement;
  let ctx: CanvasRenderingContext2D;

  onMount(() => {
    ctx = canvas.getContext('2d')!;
    drawMindMap();
  });

  function drawMindMap() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    
    // Draw center node
    ctx.beginPath();
    ctx.arc(250, 250, 30, 0, 2 * Math.PI);
    ctx.fillStyle = '#4CAF50';
    ctx.fill();

    // Draw branches and end nodes
    $branches.forEach(branch => {
      ctx.beginPath();
      ctx.moveTo(branch.x1, branch.y1);
      ctx.quadraticCurveTo(
        (branch.x1 + branch.x2) / 2,
        (branch.y1 + branch.y2) / 2 - 50,
        branch.x2,
        branch.y2
      );
      ctx.strokeStyle = '#333';
      ctx.lineWidth = 2;
      ctx.stroke();

      // Draw end node
      ctx.beginPath();
      ctx.arc(branch.x2, branch.y2, 20, 0, 2 * Math.PI);
      ctx.fillStyle = '#2196F3';
      ctx.fill();

      // Draw text along the branch
      ctx.fillStyle = '#000';
      ctx.font = '14px Arial';
      ctx.textAlign = 'center';
      ctx.textBaseline = 'middle';
      const textX = (branch.x1 + branch.x2) / 2;
      const textY = (branch.y1 + branch.y2) / 2 - 25;
      ctx.fillText(branch.text, textX, textY);
    });
  }

  function handleInput(event: Event, id: number) {
    const input = event.target as HTMLInputElement;
    branches.update(bs => 
      bs.map(b => b.id === id ? { ...b, text: input.value } : b)
    );
    drawMindMap();
  }
</script>

<div class="mind-map">
  <canvas bind:this={canvas} width="500" height="500"></canvas>
  <div class="inputs">
    {#each $branches as branch (branch.id)}
      <input
        type="text"
        placeholder="Enter branch text"
        value={branch.text}
        on:input={(e) => handleInput(e, branch.id)}
      />
    {/each}
  </div>
</div>

<style>
  .mind-map {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  canvas {
    border: 1px solid #ccc;
  }
  .inputs {
    margin-top: 1rem;
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }
</style>