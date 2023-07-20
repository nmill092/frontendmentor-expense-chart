<script>
  export let containerH, containerW;
</script>

<div
  class="container"
  bind:clientWidth={containerW}
  bind:clientHeight={containerH}
>
  <h2>Spending – Last 7 days</h2>
  <slot name="chart" />
</div>

<style lang="scss">
  @import '../styles/_variables.scss';
  .container {
    padding: 2rem 30px;
    border-radius: $br;
    background-color: $pale-orange;
    height: 100%;
    width: 100%;
  }
</style>
