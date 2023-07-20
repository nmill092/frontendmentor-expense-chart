# Frontend Mentor - Expenses chart component solution

This is a solution to the [Expenses chart component challenge on Frontend Mentor](https://www.frontendmentor.io/challenges/expenses-chart-component-e7yJBUdjwt). Frontend Mentor challenges help you improve your coding skills by building realistic projects. 

## Table of contents

- [Overview](#overview)
  - [The challenge](#the-challenge)
  - [Screenshot](#screenshot)
  - [Links](#links)
- [My process](#my-process)
  - [Built with](#built-with)
  - [What I learned](#what-i-learned)

## Overview

### The challenge

Users should be able to:

- View the bar chart and hover over the individual bars to see the correct amounts for each day
- See the current day’s bar highlighted in a different colour to the other bars
- View the optimal layout for the content depending on their device’s screen size
- See hover states for all interactive elements on the page
- **Bonus**: Use the JSON data file provided to dynamically size the bars on the chart

### Screenshot

<img width="600" alt="Expense Chart solution" src="https://github.com/nmill092/frontendmentor-expense-chart/assets/83549283/dedb56f0-0119-49e2-b711-b8c287cdafb1">

### Links

- Solution URL: [Frontendmentor](https://www.frontendmentor.io/solutions/expense-chart-component-with-svelte-d3-AD0aidbi52)
- Live Site URL: [https://frontendmentor-expense-chart.pages.dev/](https://frontendmentor-expense-chart.pages.dev/)

## My process

### Built with

- Svelte
- SCSS
- CSS Grid
- D3.js (for chart visualization)

### What I learned

A few takeaways: 
- Svelte's element bindings (specifically, `bind:clientWidth` and `bind:clientHeight`) were instrumental in ensuring the responsiveness of the chart. By linking the width of the container element to the D3-generated linear and band scales, I ensure that the bars and labels scale correctly at different screen sizes.
- CSS `clamp` was a great way to create a responsive layout without writing media queries.
- Out of the box, Svelte transitions are only triggered when a component is added to or removed from the DOM after its first render. I wanted the entire component wrapper to animate in on first render. A hack that worked out for me was 1) creating a boolean variable called `mounted` and setting its initial value to false, 2) wrapping the component in an `{#if}` block (i.e., `{#if mounted}`), and 3) using `onMount` to switch `mounted` to `true` after the component has mounted.
