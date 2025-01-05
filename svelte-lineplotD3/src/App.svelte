<script>
  import { onMount } from 'svelte';
  import { scaleLinear } from 'd3-scale';
  import { extent } from 'd3-array';

  let canvas;
  let ctx;
  let width = 800;
  let height = 400;
  let time = 0;
  let data1 = [];
  let data2 = [];
  let intervalDuration = 100; // Initial interval duration
  let maxPoints = 100; // Maximum number of points to display

  function draw() {
    ctx.clearRect(0, 0, width, height);
    
    // Draw the x-axis and y-axis
    drawAxis();

    // Draw data1 with blue line
    ctx.beginPath();
    ctx.strokeStyle = 'blue';
    data1.forEach((d, i) => {
      const x = xScale(i);
      const y = yScale(d);
      if (i === 0) {
        ctx.moveTo(x, y);
      } else {
        ctx.lineTo(x, y);
      }
    });
    ctx.stroke();

    // Draw data2 with red line
    ctx.beginPath();
    ctx.strokeStyle = 'red';
    data2.forEach((d, i) => {
      const x = xScale(i);
      const y = yScale(d);
      if (i === 0) {
        ctx.moveTo(x, y);
      } else {
        ctx.lineTo(x, y);
      }
    });
    ctx.stroke();
  }

  function drawAxis() {
    const xAxisY = height - 40;

    // Draw the x-axis
    ctx.beginPath();
    ctx.moveTo(40, xAxisY);
    ctx.lineTo(width - 40, xAxisY);
    ctx.strokeStyle = 'black';
    ctx.stroke();

    // Draw time labels every 5 seconds
    const labelInterval = 5;
    ctx.font = '12px sans-serif';
    ctx.fillStyle = 'black';
    ctx.textAlign = 'center';
    const startTime = time - (maxPoints * 0.1); // Calculate start time based on the number of points and interval duration
    for (let i = 0; i <= width; i += (labelInterval / maxPoints) * (width - 80)) {
      const labelTime = startTime + ((i / (width - 80)) * (maxPoints * 0.1));
      ctx.fillText(labelTime.toFixed(0), 40 + i, xAxisY + 15);
      ctx.moveTo(40 + i, xAxisY - 5);
      ctx.lineTo(40 + i, xAxisY + 5);
    }
    ctx.stroke();

    // Draw the y-axis
    ctx.beginPath();
    ctx.moveTo(40, 0);
    ctx.lineTo(40, height - 40);
    ctx.strokeStyle = 'black';
    ctx.stroke();

    // Draw y-axis labels
    const yAxisLabels = [-2, -1, 0, 1, 2]; // Example labels
    ctx.font = '12px sans-serif';
    ctx.fillStyle = 'black';
    ctx.textAlign = 'right';
    yAxisLabels.forEach(label => {
      const y = yScale(label);
      ctx.fillText(label, 35, y + 5);
      ctx.moveTo(35, y);
      ctx.lineTo(45, y);
    });
    ctx.stroke();
  }

  function updateData() {
    data1.push(Math.sin(time));
    data2.push(Math.cos(time));

    if (data1.length > maxPoints) {
      data1.shift(); // Keep the data array manageable
      data2.shift();
    }

    draw();
    time += 0.1;
  }

  function doubleTimePoints() {
    intervalDuration /= 2; // Double the number of time points (halve the interval duration)
    clearInterval(interval);
    interval = setInterval(updateData, intervalDuration);
  }

  let xScale, yScale;
  let interval;
  onMount(() => {
    canvas = document.getElementById('myCanvas');
    ctx = canvas.getContext('2d');

    xScale = scaleLinear()
      .domain([0, maxPoints])
      .range([40, width - 40]);

    yScale = scaleLinear()
      .domain([-2, 2])
      .range([height - 40, 40]);

    interval = setInterval(updateData, intervalDuration);

    return () => clearInterval(interval);
  });
</script>

<main>
  <canvas id="myCanvas" width={width} height={height}></canvas>
  <button on:click={doubleTimePoints}>Double Time Points</button>
</main>

<style>
  canvas {
    display: block;
    margin: 0 auto;
    border: 1px solid #ccc;
  }

  button {
    display: block;
    margin: 20px auto;
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 5px;
  }

  button:hover {
    background-color: #0056b3;
  }
</style>
