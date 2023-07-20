<script>
  import './global.scss';
  import './lib/SpendingContainer.svelte';
  import BalanceContainer from './lib/BalanceContainer.svelte';
  import SpendingContainer from './lib/SpendingContainer.svelte';
  import ExpenseChart from './lib/ExpenseChart.svelte';
  import { fade, fly, slide } from 'svelte/transition';

  let containerH, containerW; 
</script>

<main class="wrapper">
  <BalanceContainer/>
  <SpendingContainer bind:containerH bind:containerW>
    <ExpenseChart {containerH} {containerW} slot="chart"/>
  </SpendingContainer>
</main>



<style lang="scss">
  @import "./_variables";

  .wrapper {
    height: clamp(60vh, 660px, 90vh);
    width: clamp(30vw, 540px, 90vw);
    display: grid; 
    grid-template-rows: .3fr 1fr;
    grid-gap: 1.3rem;
  }
</style>