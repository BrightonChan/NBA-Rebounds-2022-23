<script>
  export let xAccessorScaled;
  export let yAccessorScaled;
  export let tooltipText;
  export let marginLeft;
  export let marginTop;

  $: x = xAccessorScaled + marginLeft;
  $: y = yAccessorScaled + marginTop;
</script>

<div
  class="tooltip"
  style="transform: translate(calc(-50% + {x}px), calc(-100% + {y}px))"
>
  <div>{tooltipText}</div>
</div>

<style>
  .tooltip {
    display: flex;
    flex-direction: column;
    background: #ffffff;
    position: absolute;
    top: -12px;
    left: 0;
    padding: 0.2em;
    pointer-events: none;
    color: #dc267f;
  }
</style>
