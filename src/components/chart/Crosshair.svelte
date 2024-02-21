<script>
  export let xAccessorScaled;
  export let yAccessorScaled;
  export let xLabel;
  export let yLabel;
  export let boundedHeight;
</script>

<g transform={`translate(${xAccessorScaled} 0)`}>
  <text
    class="crosshair__text-outline"
    y={boundedHeight + 10}
    dy="0.8em"
    text-anchor="middle"
  >
    {xLabel}
  </text>

  <text
    class="crosshair__text"
    y={boundedHeight + 10}
    dy="0.8em"
    text-anchor="middle"
  >
    {xLabel}
  </text>

  <line
    class="crosshair__line-outline"
    x1={0}
    x2={0}
    y1={yAccessorScaled}
    y2={boundedHeight}
  />

  <line
    class="crosshair__line"
    x1={0}
    x2={0}
    y1={yAccessorScaled}
    y2={boundedHeight}
  />
</g>

<g transform={`translate(0 ${yAccessorScaled})`}>
  <text class="crosshair__text-outline" dx={-10} dy="0.34em" text-anchor="end">
    {yLabel}
  </text>

  <text class="crosshair__text" dx={-10} dy="0.34em" text-anchor="end">
    {yLabel}
  </text>

  <line
    class="crosshair__line-outline"
    x1={xAccessorScaled}
    x2={0}
    y1={0}
    y2={0}
  />

  <line class="crosshair__line" x1={xAccessorScaled} x2={0} y1={0} y2={0} />
</g>

<style>
  .crosshair__text-outline {
    stroke: #ffffff;
    stroke-width: 8;
  }

  .crosshair__text {
    fill: #dc267f;
  }

  .crosshair__line-outline {
    stroke: #ffffff;
    stroke-width: 3;
  }

  .crosshair__line {
    stroke: #dc267f;
    stroke-width: 1;
  }
</style>
