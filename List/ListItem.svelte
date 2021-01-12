<script>
  let ref;

  export let item;
</script>

<div bind:this={ref} >
  <slot {ref} />
</div>