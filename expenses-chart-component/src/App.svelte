<script>
  import { easeExpInOut } from "d3";
  import { onMount } from "svelte";
  import { fly } from "svelte/transition";
  import "./global.scss";
  import BalanceContainer from "./lib/BalanceContainer.svelte";
  import ExpenseChart from "./lib/ExpenseChart.svelte";
  import SpendingContainer from "./lib/SpendingContainer.svelte";

  let mounted = false;
  let containerH, containerW;

  onMount(() => {
    mounted = true;
  }); // hack to enable transition on first render
</script>

{#if mounted}
  <main
    class="wrapper"
    transition:fly={{
      y: 1000,
      duration: 1000,
      opacity: 0.2,
      easing: easeExpInOut,
    }}
  >
    <BalanceContainer />
    <SpendingContainer bind:containerH bind:containerW>
      <ExpenseChart {containerH} {containerW} slot="chart" />
    </SpendingContainer>
  </main>
{/if}

<style lang="scss">
  @import "src/_variables.scss";

  .wrapper {
    height: clamp(60vh, 660px, 90vh);
    width: clamp(30vw, 540px, 90vw);
    display: grid;
    grid-template-rows: 0.3fr 1fr;
    grid-gap: 1.3rem;
  }
</style>
