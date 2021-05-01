<script>
  import {createEventDispatcher} from 'svelte';
  const dispatch = createEventDispatcher();

  export let started;
  export let running;

  function start() {
    dispatch("startTiming");
  }

  function pause() {
    dispatch("pauseTiming");
  }
</script>

<style>
  /* display the button(s) in a row */
  div {
      display: flex;
  }
  div button:focus {
      outline-color: hsl(0, 0%, 25%);
  }
  div button:hover {
      background: hsl(0, 0%, 92%);
  }
</style>

<div class="controls">
  {#if started && running}
    <button class="btn btn__relevant" on:click="{pause}">Pause</button>
  {:else if started && !running}
    <button class="btn" on:click="{start}">Continue</button>
  {:else}
    <button class="btn" on:click="{start}">Start</button>
  {/if}
</div>