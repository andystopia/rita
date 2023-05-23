<script lang="ts">
  import { fade, slide } from "svelte/transition";
  import {
    bounceIn,
    circIn,
    cubicIn,
    expoIn,
    expoInOut,
    quartIn,
  } from "svelte/easing";
  import { onMount } from "svelte";

  let intro = true;
  let innerText = "Focus With Rita.";
  function typewriter(node: HTMLDivElement, { speed = 1 }: { speed: number }) {
    const valid =
      node.childNodes.length === 1 &&
      node.childNodes[0].nodeType === Node.TEXT_NODE;

    if (!valid) {
      throw new Error(
        `This transition only works on elements with a single text node child`
      );
    }

    const text = node.textContent;
    const duration = text.length / (speed * 0.01);

    return {
      duration,
      tick: (t) => {
        console.log(t);
        const i = Math.trunc(text.length * cubicIn(t));
        node.textContent = text.slice(0, i);
        var sel = window.getSelection();
        var range = document.createRange();
        range.selectNodeContents(node);
        range.collapse(false);
        sel.removeAllRanges();
        sel.addRange(range);
      },
    };
  }
  onMount(() => {
    intro = false;
  });
</script>

<main class="container">
  <div class="dragregion" data-tauri-drag-region />
  <div class="privacy" />
  {#if intro}
    <div
      class="editfield"
      in:typewriter={{ speed: 2 }}
      on:introend={() => {
        intro = false;
      }}
    >
      Rita
    </div>
  {:else}
    <span in:fade>
      <div
        contenteditable
        class="editfield"
        autofocus
        bind:innerHTML={innerText}
        in:typewriter={{ speed: 1.5 }}
      /></span
    >
  {/if}
</main>

<style>
  .dragregion {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    z-index: 3;
    height: 35px;
    background: var(--bg-color);
  }
  .privacy {
    position: absolute;
    width: 100%;
    height: calc(50% - 1em - 0.2em);
    z-index: 2;
    background: linear-gradient(
      180deg,
      var(--bg-color) 0%,
      var(--bg-color) calc(100% - 1em),
      var(--bg-color) calc(100% - 2.2em),
      #050505cc calc(100% - 1em),
      #05050570 00 100%
    );
  }
  .editfield {
    outline: none;
    width: 20em;
    z-index: 0;

    --computed-height: 2em;
    line-height: 1.2em;
    position: absolute;
    left: 50%;
    transform: translate(-50%, 0);
    bottom: calc(50%);
    text-justify: inter-word;
    text-align: justify;
    text-align-last: center;
    opacity: 0.5;
  }

  .container {
    position: relative;
    margin: 0;
    width: 100%;
    align-items: center;
    height: 100%;
    text-align: center;
  }
</style>

