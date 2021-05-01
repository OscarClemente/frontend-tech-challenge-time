<script>
  import {operationStore, query} from '@urql/svelte';
  import Timer from './Timer.svelte';
  import NewTimer from './NewTimer.svelte';

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

  function addTimer(newTimer) {
    timers = [...timers, {id: newTimer.id, title: newTimer.title, timeElapsed: newTimer.timeElapsed, createdAt: newTimer.createdAt}];
  }

  function deleteTimer(timer) {
    timers = timers.filter(t => t.id !== timer.id);
  }  
</script>

<div class="timerapp stack-large">
  <!-- NewTimer-->
  <NewTimer on:addTimer={e => addTimer(e.detail)} />

  <!-- Timers -->
  <ul role="list" class="timer-list stack-large" aria-labelledby="list-heading">
    {#each timers as timer (timer.id)}
      <li class="timer">
        <Timer {timer}
          on:delete={e => deleteTimer(e.detail)}
        />
      </li>
    {:else}
      <li>
        No timers saved!
    {/each}
  <ul>
</div>