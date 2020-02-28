---
layout: post
title: Svelte.JS Project Base Tutorial Mortgage Calculator
date: 2020-02-28 14:17 -0800
categories: ['svelte', 'javascript', 'reactive variables', 'binding input']
---

<iframe width="560" height="315" src="https://www.youtube.com/embed/n2BiGntzVWY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Demo

[demo link](https://phptuts.github.io/svelte-mortgage-calc-yt/)

![demo mortgage calculator](/assets/2020-02-28-svelte-mortgage-calculator-reactive-variable/demo.png)

## Svelte Example

[Reactive Variables](https://svelte.dev/examples#reactive-declarations)

[Text Binding](https://svelte.dev/examples#text-inputs)

## Revelant Links

- [Svelte](https://svelte.dev/)
- [Skelton CSS](http://getskeleton.com/)
- [Skelton CSS CDN](https://cdnjs.com/libraries/skeleton)
- [JS Money Formatter](https://stackoverflow.com/a/16233919)
- [Intl Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NumberFormat)
- [Github Pages Node Modules](https://www.npmjs.com/package/gh-pages)
- [Math Monthly Payments](https://math.stackexchange.com/a/668162)

## Code

[code](https://github.com/phptuts/svelte-mortgage-calc-yt)

```html
<script>
  var formatter = new Intl.NumberFormat("en-US", {
    style: "currency",
    currency: "USD"
  });
  let loanAmount = 200000;
  let years = 15;
  let interestRateInput = 200;
  $: interestRate = interestRateInput / 100;
  $: totalPayments = years * 12;
  $: monthlyInterestRate = interestRate / 100 / 12;
  $: monthlyPayment =
    (loanAmount *
      Math.pow(1 + monthlyInterestRate, totalPayments) *
      monthlyInterestRate) /
    (Math.pow(1 + monthlyInterestRate, totalPayments) - 1);
  $: totalPaid = monthlyPayment * totalPayments;
  $: interestPaid = totalPaid - loanAmount;
</script>

<style>
  .outputs {
    font-size: 20px;
    border: solid black 2px;
    margin-top: 15px;
    text-align: center;
  }
</style>

<main class="container">
  <div class="row">
    <h1>Mortgage Calculator</h1>
  </div>
  <div class="row">
    <label>Loan Amount</label>
    <input
      min="1"
      bind:value={loanAmount}
      placeholder="Enter loan amount"
      type="number"
      class="u-full-width" />
  </div>
  <div class="row">
    <div class="columns six">
      <label>Years</label>
      <input
        type="range"
        min="1"
        max="50"
        class="u-full-width"
        bind:value={years} />
    </div>
    <div class="columns six outputs">{years} years</div>
  </div>
  <div class="row">
    <div class="columns six">
      <label>Interest Rate</label>
      <input
        type="range"
        min="1"
        max="2000"
        class="u-full-width"
        bind:value={interestRateInput} />
    </div>
    <div class="columns six outputs">{interestRate.toFixed(2)}%</div>
  </div>

  <div class="row outputs">
    Monthly Payments {formatter.format(monthlyPayment)}
  </div>
  <div class="row outputs">Total Paid {formatter.format(totalPaid)}</div>
  <div class="row outputs">Interest Paid {formatter.format(interestPaid)}</div>

</main>
```
