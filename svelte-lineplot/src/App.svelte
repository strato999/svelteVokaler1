<script>
  import { onMount } from 'svelte';

  let canvas;
  let ctx;
  let width = 800;
  let height = 400;
  let time = 0;
  let data1 = [];
  let data2 = [];

  function draw() {
    ctx.clearRect(0, 0, width, height);

    // Draw data1 with blue line
    ctx.beginPath();
    ctx.strokeStyle = 'blue';
    for (let i = 0; i < data1.length; i++) {
      const x = (i / data1.length) * width;
      const y = height / 2 - data1[i] * 50; // Adjust scale as needed
      if (i === 0) {
        ctx.moveTo(x, y);
      } else {
        ctx.lineTo(x, y);
      }
    }
    ctx.stroke();

    // Draw data2 with red line
    ctx.beginPath();
    ctx.strokeStyle = 'red';
    for (let i = 0; i < data2.length; i++) {
      const x = (i / data2.length) * width;
      const y = height / 2 - data2[i] * 50; // Adjust scale as needed
      if (i === 0) {
        ctx.moveTo(x, y);
      } else {
        ctx.lineTo(x, y);
      }
    }
    ctx.stroke();
  }

  onMount(() => {
    canvas = document.getElementById('myCanvas');
    ctx = canvas.getContext('2d');

    const interval = setInterval(() => {
      // Update data
      data1.push(Math.sin(time));
      data2.push(Math.cos(time));

      if (data1.length > 100) {
        data1.shift(); // Keep the data array manageable
        data2.shift();
      }

      draw();
      time += 0.1;
    }, 100);

    return () => clearInterval(interval);
  });
</script>

<canvas id="myCanvas" width={width} height={height}></canvas>

<style>
  canvas {
    display: block;
    margin: 0 auto;
    border: 1px solid #ccc;
  }
</style>
