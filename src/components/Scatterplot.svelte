<script>
  import { setContext } from 'svelte';
  import { writable } from 'svelte/store';
  import * as d3 from 'd3';
  import XAxis from './chart/XAxis.svelte';
  import YAxis from './chart/YAxis.svelte';
  import Crosshair from './chart/Crosshair.svelte';
  import Tooltip from './chart/Tooltip.svelte';

  export let data;
  export let selectedPlayer;
  export let selectedPosition;
  let filterPlayers;

  $: filteredData = data.filter(
    function(d){
      if (selectedPosition == "allPositions") {
        return d
      } else {
        return d.Pos == selectedPosition
      }
    });

   
  $: selectedPoint = data.find(({ player }) => player === selectedPlayer);

  const textAccessor = d => d.player;
  const xAccessor = d => +d.Minutes_Played;
  const yAccessor = d => +d.Rebounds;
  const xExtent = d3.extent(data, xAccessor);
  const yExtent = d3.extent(data, yAccessor);

  const formatTick = d3.format('.2s');
  const formatLabel = d3.format('.1f');

  const pointColor = '#9733FF';
  const onepointColor = '#FF3933'
  const selectedPointColor = '#FF92N3';
  const hoveredPointColor = '#dc267f';
  const white = '#FFFFFF';

  let hoveredPoint = null;

  let width = 100;
  $: height = 0.7 * width;

  const margins = {
    marginTop: 40,
    marginRight: 55,
    marginBottom: 65,
    marginLeft: 55,
  };

  $: dimensions = {
    width,
    height,
    ...margins,
    boundedHeight: Math.max(
      height - margins.marginTop - margins.marginBottom,
      0,
    ),
    boundedWidth: Math.max(width - margins.marginLeft - margins.marginRight, 0),
  };

  $: xScale = d3
    .scaleLinear()
    .domain(padExtent(xExtent, 0.1))
    .range([0, dimensions.boundedWidth])
    .nice();

  $: yScale = d3
    .scaleLinear()
    .domain(padExtent(yExtent, 0.1))
    .range([dimensions.boundedHeight, 0])
    .nice();

  $: xAccessorScaled = d => xScale(xAccessor(d));
  $: yAccessorScaled = d => yScale(yAccessor(d));

  let currentDimensions = writable(dimensions);

  $: {
    currentDimensions.set(dimensions);
  }

  setContext('chart', {
    dimensions: currentDimensions,
  });

  $: quadtreeInstance = d3
    .quadtree()
    .x(d => xScale(xAccessor(d)))
    .y(d => yScale(yAccessor(d)))
    .addAll(data);

  const handleMouseMove = event => {
    const [mouseX, mouseY] = d3.pointer(event);
    // Check if the pointer is close to the selected player
    const selectedPlayerPoint = data.find(d => d.player === selectedPlayer);

    if (selectedPlayerPoint) {
      const [selectedPlayerX, selectedPlayerY] = [
        xAccessorScaled(selectedPlayerPoint),
        yAccessorScaled(selectedPlayerPoint),
      ];

      const distanceToSelectedPlayer = Math.sqrt(
        Math.pow(mouseX - selectedPlayerX, 2) + Math.pow(mouseY - selectedPlayerY, 2)
      );

      // Set the tooltip to the selected player if close enough
      if (distanceToSelectedPlayer <= 60) { // Adjust the threshold as needed
        hoveredPoint = selectedPlayerPoint;
        return;
      }
    }
    hoveredPoint = quadtreeInstance.find(mouseX, mouseY);
  };

  const handleMouseLeave = () => {
    hoveredPoint = null;
  };

  const padExtent = ([min, max], paddingFactor) => {
    const delta = Math.abs(max - min);
    const padding = delta * paddingFactor;

    return [min - padding, max + padding];
  };
</script>

<div class="wrapper" bind:clientWidth={width} style="height: {height}px">
  <svg class="chart" width={dimensions.width} height={dimensions.height}>
    <g
      transform={`translate(${dimensions.marginLeft}, ${dimensions.marginTop})`}
    >
      <XAxis
        scale={xScale}
        label="Minutes Per Game"
        {formatTick}
        {hoveredPoint}
      />

      <YAxis
        scale={yScale}
        label="Rebounds"
        {formatTick}
        {hoveredPoint}
      />

      <rect
        width={dimensions.boundedWidth}
        height={dimensions.boundedHeight}
        x={0}
        y={0}
        fill="#ffffff"
        fill-opacity={0}
        role="button"
        tabindex="0"
        on:mousemove={handleMouseMove}
        on:mouseleave={handleMouseLeave}
      />

      {#each data as d}
        <circle
          class="chart__point"
          r="7"
          cx={xAccessorScaled(d)}
          cy={yAccessorScaled(d)}
          fill={pointColor}
        />
      {/each}
      
      {#if selectedPosition}
        {#each data as d}
          <circle
            class="chart__point"
            r="7"
            cx={xAccessorScaled(d)}
            cy={yAccessorScaled(d)}
            fill={white}
          />
        {/each}
        {#each filteredData as fd}
          <circle
            class="chart__point"
            r="7"
            cx={xAccessorScaled(fd)}
            cy={yAccessorScaled(fd)}
            fill={pointColor}
          />
        {/each}
      {/if}

      {#if selectedPoint}
        <circle
          class="chart__point"
          r="7"
          cx={xAccessorScaled(selectedPoint)}
          cy={yAccessorScaled(selectedPoint)}
          fill={onepointColor}
        />
      {/if}
      

      {#if hoveredPoint}
        <Crosshair
          xAccessorScaled={xAccessorScaled(hoveredPoint)}
          yAccessorScaled={yAccessorScaled(hoveredPoint)}
          r="0.2"
          xLabel={formatLabel(xAccessor(hoveredPoint))}
          yLabel={formatLabel(yAccessor(hoveredPoint))}
          boundedHeight={dimensions.boundedHeight}
        />

        <circle
          class="chart__point"
          r="7"
          cx={xAccessorScaled(hoveredPoint)}
          cy={yAccessorScaled(hoveredPoint)}
          fill={hoveredPointColor}
        />
      {/if}
    </g>
  </svg>

  {#if hoveredPoint}
    <Tooltip
      xAccessorScaled={xAccessorScaled(hoveredPoint)}
      yAccessorScaled={yAccessorScaled(hoveredPoint)}
      tooltipText={textAccessor(hoveredPoint)}
      marginLeft={margins.marginLeft}
      marginTop={margins.marginTop}
    />
  {/if}
</div>

<style>
  .wrapper {
    position: relative;
    font-size: 16px;
    width: 100%;
    max-width: 600px;
    margin: 0 40px;
  }

  .chart__point {
    stroke: #ffffff;
    stroke-width: 1px;
  }
</style>
