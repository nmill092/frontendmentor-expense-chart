<script>
  export let containerH, containerW;
  import { max, scaleBand, scaleLinear } from "d3";
  import { fade, fly } from "svelte/transition";
  import { onMount } from "svelte";
  import data from "../assets/data.json";
  let amount;
  let mounted = false;
  const config = { mb: 20, mt: 30, ml: 30, mr: 30 };

  let barStyles = {
    active: "hsl(10, 79%, 65%)",
    today: "hsl(186, 34%, 60%)",
    inactive: "#EC755D",
  };

  $: xScale = scaleBand()
    .domain(data.map((d) => d.day))
    .range([0, containerW - 60])
    .padding(0.2);

  $: yScale = scaleLinear()
    .domain([0, max(data, (d) => d.amount)])
    .range([containerH / 2 - config.mb, config.mt]);

  let popupPosition = { x: 0, y: 0 };

  let dayArray = data.map((d) => d.day);
  let today = dayArray[new Date().getDay() - 1];

  onMount(() => (mounted = true));
</script>

<div class="chart-container">
  <svg width={"100%"} height={containerH / 2}>
    {#each data as d, i}
      {#if mounted}
        <g transition:fade={{ delay: 500 + i * 100, duration: 500 }}>
          <rect
            on:focus={() => (amount = d.amount)}
            on:mouseover={(e) => {
              amount = d.amount;
              popupPosition = { x: xScale(d.day), y: yScale(d.amount) };
            }}
            on:mouseleave={() => (amount = null)}
            class="expense-bar"
            rx={5}
            x={xScale(d.day)}
            y={yScale(d.amount)}
            style={`fill: ${
              d.day === today ? barStyles.today : barStyles.inactive
            }`}
            height={yScale(0) - yScale(d.amount)}
            width={xScale.bandwidth()}
          />
          <text
            class="expense-tick"
            x={xScale(d.day) + xScale.bandwidth() / 2}
            y={containerH / 2}>{d.day}</text
          >
        </g>
      {/if}
    {/each}
  </svg>

  <hr class="expense-divider" />

  <div class="bottom-row">
    <div class="total">
      <span class="total-header">Total this month</span>
      <span class="total-value"
        >${data.reduce((acc, obj) => acc + obj.amount, 0)}</span
      >
    </div>
    <div class="month">
      <span>+2.4%</span>
      <span>from last month</span>
    </div>
  </div>

  {#if amount}
    <div
      style="left: {popupPosition.x}px; top: {popupPosition.y}px;"
      class="popup"
      transition:fly={{ y: 50, opacity: 0 }}
    >
      <span>${amount}</span>
    </div>
  {/if}
</div>

<style lang="scss">
  @use "sass:color";
  @import "../variables";

  .chart-container {
    width: 100%;
    display: grid;
    grid-gap: 2rem;
    height: clamp(3rem, 5rem, 6rem);
    grid-template-rows: 1fr 2fr 0.5fr 1fr;
  }

  .expense-bar {
    cursor: pointer;
    fill: $red;
    transition: 0.3s ease opacity;
    &:hover {
      opacity: 0.5;
    }
  }
  .expense-divider {
    border: 2px solid $divider;
    border-radius: $br;
  }
  .expense-tick {
    fill: $med-brown;
    font-size: clamp(0.7rem, $root-fs/1.2, 3vw);
    text-anchor: middle;
  }

  .popup {
    transform: translate(20px, 25px);
    border-radius: 10px;
    padding: 0.4rem 0.8rem;
    font-weight: $fw-bold;
    background-color: color.adjust($dark-brown, $alpha: -0.1);
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
