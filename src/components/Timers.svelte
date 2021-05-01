<script>
  import {operationStore, query} from '@urql/svelte'

  export let timers = [];

  const getTimersStore = operationStore(
    `
    query {
      timers {
        id,
        title,
        timeElapsed,
        createdAt,
      }
    }
    `, undefined, {pause: false}
  );

  query(getTimersStore);
  let timersLoaded = false;

  $: {
    if ($getTimersStore.fetching == false && timersLoaded == false) {
      $getTimersStore.context.pause = true;
      timersLoaded = true;
      for (let timer of getTimersStore.data.timers) {
        var createdAt = new Date(timer.date);
        timers = [...timers, {id: timer.id, title: timer.title, timeElapsed: timer.timeElapsed, createdAt: timer.createdAt}];
      }
    }
  }
  
</script>

<div class="timerapp stack-large">
  <!-- Timers -->
  <ul role="list" class="timer-list stack-large" aria-labelledby="list-heading">
    {#each timers as timer (timer.id)}
      <li class="timer">
        {timer.id} - {timer.title}
      </li>
    {:else}
      <li>
        No timers saved!
    {/each}
  <ul>
</div>