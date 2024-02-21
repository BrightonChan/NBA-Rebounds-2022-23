<script>
  import { getContext } from 'svelte';
  export let scale;
  export let label;
  export let formatTick;
  export let hoveredPoint;

  const { dimensions: dimensionsStore } = getContext('chart');
  $: dimensions = $dimensionsStore;

  const numberOfTicks = (pixelsAvailable, pixelsPerTick = 140) =>
    Math.floor(Math.abs(pixelsAvailable) / pixelsPerTick);

  $: [xMin, xMax] = scale.range();

  $: ticks = scale.ticks(numberOfTicks(xMax - xMin));
</script>

<g transform={`translate(0 ${dimensions.boundedHeight})`}>
  <line class="axis__line" x1={xMin} x2={xMax} y1={0} y2={0} />

  {#each ticks as tick}
    <g transform={`translate(${scale(tick)} 0)`}>
      <line class="axis__tick" y1={0} y2={6} />/>

      <text
        y={10}
        dy="0.8em"
        text-anchor="middle"
        fill={hoveredPoint ? '#bdc3c7' : '#282828'}
      >
        {formatTick(tick)}
      </text>
    </g>
  {/each}

  <text
    class="axis__label"
    x={scale.range()[1] / 2}
    text-anchor="middle"
    y={50}
  >
    {label}
  </text>
</g>

<style>
  .axis__line {
    stroke: #bdc3c7;
  }

  .axis__tick {
    stroke: #bdc3c7;
  }

  .axis__label {
    fill: #000000;
  }
</style>
