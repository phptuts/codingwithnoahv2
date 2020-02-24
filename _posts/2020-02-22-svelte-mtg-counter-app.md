---
layout: post
title: Svelte JS Magic the Gathering App
date: 2020-02-22 22:38 -0800
categories: ['svelte', 'javascript', 'reactive variables']
---

<iframe width="560" height="315" src="https://www.youtube.com/embed/M_teofI6vKY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Code 

[REPL Example](https://svelte.dev/repl/74773d95ebfd4a49bc02d9d40533ab8d?version=3.19.1)

[Github MTG Code](https://github.com/phptuts/svelte-mtg-yt)



### App Component

```html
<script>
  import { createEventDispatcher } from "svelte";
  export let score;
  export let winningText;
  export let won;
  export let fontColor;
  export let gameOver = false;
  const dispatch = createEventDispatcher();
  function minus() {
    dispatch("points", -1);
  }
  function plus() {
    dispatch("points", 1);
  }
</script>

<style>
  .player {
    flex-grow: 1;
  }
  .plus {
    background-color: seagreen;
  }
  .minus {
    background-color: brown;
  }
  button[disabled] {
    background-color: gray;
    pointer-events: none;
  }
  button {
    font-size: 20px;
    border-radius: 3px;
    width: 40px;
    color: white;
    font-family: monospace;
    font-weight: bold;
  }
</style>

<div style="color: {fontColor}" class="player">
  <h2>{score}</h2>
  <button on:click={plus} disabled={gameOver} class="plus">+</button>
  <button on:click={minus} disabled={gameOver} class="minus">-</button>
  {#if won}
    <h2>{winningText}</h2>
  {/if}
</div>

```

### Player Component Code

```html
<script>
  import { createEventDispatcher } from "svelte";
  export let score;
  export let winningText;
  export let won;
  export let fontColor;
  export let gameOver = false;
  const dispatch = createEventDispatcher();
  function minus() {
    dispatch("points", -1);
  }
  function plus() {
    dispatch("points", 1);
  }
</script>

<style>
  .player {
    flex-grow: 1;
  }
  .plus {
    background-color: seagreen;
  }
  .minus {
    background-color: brown;
  }
  button[disabled] {
    background-color: gray;
    pointer-events: none;
  }
  button {
    font-size: 20px;
    border-radius: 3px;
    width: 40px;
    color: white;
    font-family: monospace;
    font-weight: bold;
  }
</style>

<div style="color: {fontColor}" class="player">
  <h2>{score}</h2>
  <button on:click={plus} disabled={gameOver} class="plus">+</button>
  <button on:click={minus} disabled={gameOver} class="minus">-</button>
  {#if won}
    <h2>{winningText}</h2>
  {/if}
</div>
© 2020 GitHub, Inc.
```

## Demo

We are building a magic the gathering counter app.  It allows the user to keep
track of the score of their magic game. Here is the working
[demo](https://phptuts.github.io/svelte-mtg-yt/)

![demo](/assets/2020-02-22-svelte-mtg-counter-app/demo.png)

## Revelant Tutorials 

- [Components](https://svelte.dev/examples#nested-components)
- [Props](https://svelte.dev/examples#declaring-props)
- [Reactive Variables](https://svelte.dev/examples#reactive-declarations)
- [Template Logic](https://svelte.dev/examples#if-blocks)
- [DOM Events](https://svelte.dev/examples#dom-events)
- [Component Events](https://svelte.dev/examples#component-events)


## Requirements

- [nodejs](http://nodejs.org)
- [git](https://git-scm.com/)
- [github account](http://github.com/)
- [github ssh keys](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
- [vs code](https://code.visualstudio.com/) Optional but recommended
- [vs code plugin](https://marketplace.visualstudio.com/items?itemName=JamesBirtles.svelte-vscode)
  Optional but recommended
- [github pages](https://www.npmjs.com/package/gh-pages)



