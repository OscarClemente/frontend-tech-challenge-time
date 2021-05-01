<script>
  export let timers = []
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