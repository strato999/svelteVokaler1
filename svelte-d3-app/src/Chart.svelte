<script>
  import * as d3 from 'd3';
  import { onMount } from 'svelte';

  let data = [];
  let label = '';

  onMount(async () => {
    const response = await fetch('/data.txt');
    const text = await response.text();
    const lines = text.trim().split('\n');

    // Extract the label from the header
    const header = lines[0].split(' ');
    label = `${header[2]} versus Time`;

    // Parse the data
    const dataLines = lines.slice(1); // Skip header
    data = dataLines.map(line => {
      const [time, elapsedTime, memory] = line.split(' ');
      return {
        date: new Date(time.replace('_', ' ')),
        elapsedTime: parseFloat(elapsedTime),
        memory: parseFloat(memory),
      };
    });

    drawChart();
  });

  function drawChart() {
    const svg = d3.select('svg');
    const margin = { top: 40, right: 20, bottom: 30, left: 50 };
    const width = +svg.attr('width') - margin.left - margin.right;
    const height = +svg.attr('height') - margin.top - margin.bottom;
    const g = svg.append('g').attr('transform', `translate(${margin.left},${margin.top})`);

    // Add the chart label
    svg.append('text')
      .attr('x', width / 2)
      .attr('y', margin.top / 2)
      .attr('text-anchor', 'middle')
      .style('font-size', '16px')
      .style('text-decoration', 'underline')
      .text(label);

    const x = d3.scaleTime().domain(d3.extent(data, d => d.date)).range([0, width]);
    const y = d3.scaleLinear().domain(d3.extent(data, d => d.memory)).range([height, 0]);

    g.append('g')
      .attr('transform', `translate(0,${height})`)
      .call(d3.axisBottom(x));

    g.append('g')
      .call(d3.axisLeft(y));

    g.selectAll('.dot')
      .data(data)
      .enter().append('circle')
      .attr('class', 'dot')
      .attr('cx', d => x(d.date))
      .attr('cy', d => y(d.memory))
      .attr('r', 3);
  }
</script>

<style>
  .dot {
    fill: steelblue;
  }
</style>

<svg width="800" height="400"></svg>
