<script>
  export let containerH, containerW;

  import { max, scaleBand, scaleLinear } from "d3";
  import data from "../assets/data.json";
  console.log(data.reduce((a, b) => 0 + b.amount, 0))
  import { fade } from "svelte/transition";
  let amount;
  const config = { mb: 20, mt: 30 };

  $: xScale = scaleBand()
    .domain(data.map((d) => d.day))
    .range([0, (containerW-60)])
    .padding(0.2);

  $: yScale = scaleLinear()
    .domain([0, max(data, (d) => d.amount)])
    .range([containerH / 2 - config.mb, config.mt]);

let popupPosition = { x: 0, y: 0 }; 



</script>

<div class="chart-container">
  <svg width={"100%"} height={containerH / 2}>
    {#each data as d}
      <g>
        <rect
          on:focus={() => (amount = d.amount)}
          on:mouseover={(e) => {
            amount = d.amount;
            popupPosition = { x: xScale(d.day), y: yScale(d.amount)};
          }}
          on:mouseleave={() => (amount = null)}
          class="expense-bar"
          rx={5}
          x={xScale(d.day)}
          y={yScale(d.amount)}
          height={yScale(0) - yScale(d.amount)}
          width={xScale.bandwidth()}
        />
        <text
          class="expense-tick"
          x={xScale(d.day) + xScale.bandwidth() / 2}
          y={containerH / 2}>{d.day}</text
        >
      </g>
    {/each}
  </svg>

  <hr class="expense-divider"/>

  <div class="bottom-row">
  <div class="total">
    <span class="total-header">Total this month</span>
    <span class="total-value">${data.reduce((acc, obj) => acc+obj.amount, 0)}</span>
  </div>
  <div class="month">
    <span>+2.4%</span>
    <span>from last month</span>
  </div>
</div>

  {#if amount}
    <div style="left: {popupPosition.x}px; top: {popupPosition.y}px;"
        class="popup" transition:fade={{duration:100}}>
      <span>${amount}</span>
    </div>
  {/if}
</div>

<style lang="scss">
  @use 'sass:color';
  @import "../variables";

  .chart-container {
    width: 100%;
    display: grid;
    grid-gap: 2rem;
    height: clamp(3rem, 5rem, 6rem);
    grid-template-rows: 1fr 2fr .5fr 1fr;
   
  }

  .expense-bar {
    cursor: pointer; 
    fill: $red;
    transition: .3s ease all; 
    &:hover {
        fill: $cyan;
    }
  }
  .expense-divider {
    border: 2px solid $divider;
    border-radius: $br;
  }
  .expense-tick {
    fill: $med-brown;
    font-size: clamp(.7rem, $root-fs/1.2, 3vw);
    text-anchor: middle;
  }

  .popup {
    transform: translate(20px, 25px);
    border-radius: 10px;
    padding: .4rem .8rem;
    font-weight: $fw-bold;
    background-color: color.adjust($dark-brown, $alpha: -.1);
    color: white;
    position: absolute;
    left: 0;
    pointer-events: none; 
  }
  .total span {
    display: block;
  }
  .total-header {
    font-size: $root-fs; 
    color: $med-brown;
  }

  .total-value {
    font-size: calc($root-fs * 1.8);
    font-weight: $fw-bold;
  }

  .bottom-row {
    display: flex; 
    align-items: flex-end;
    justify-content: space-between;
  }

  .month {
    span {
        display: block;
        text-align: right;
        color: $dark-brown;
    }
    span:nth-child(1) {
        font-weight: $fw-bold;
    }
    span:nth-child(2) {
        font-weight: $fw-reg; 
    }
  }
</style>
