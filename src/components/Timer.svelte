<script>
  import {createEventDispatcher} from 'svelte';
  import {mutation} from '@urql/svelte';
  import {time} from '../stores.js';
  import {formatTime} from '../utils.js';
  import TimerControls from './TimerControls.svelte';

  export let timer;
  const dispatch = createEventDispatcher();

  const deleteTimerMutation = mutation({
    query: `
      mutation($id: ID!) {
        deleteTimer(id: $id)
      }
    `
  });

  const updateTimerMutation = mutation({
    query: `
      mutation($id: ID!, $title: String!, $timeElapsed: Int!) {
        updateTimer(input: {id: $id, title: $title, timeElapsed: $timeElapsed}) {
          id,
        }
      }
    `
  });

  // Editing title and saving variables and functions

  let editing = false;
  let title = timer.title;

  function onDelete() {
    deleteTimerMutation({id: timer.id}).then(result => {
      if (result.data.deleteTimer === true) {
        dispatch('delete', timer);
      }
    })
  }

  function onEdit() {
    editing = true;
  }

  function onCancelEdit() {
    title = timer.title;
    editing = false;
  }

  function onSave() {
    updateTimerMutation({id: timer.id, title: title, timeElapsed: timer.timeElapsed});
    timer = { ...timer, ...{title: title}};
    editing = false;
  }

  // Timing variables and functions

  $: started = false;
  $: running = false;
  let previous = timer.timeElapsed;
  let unsubscribe;

  function startTiming() {
    unsubscribe = time.subscribe(value => {
      timer.timeElapsed = value + previous;
    });
    started = true;
    running = true;
  }

  function pauseTiming() {
    if (unsubscribe) {
      unsubscribe();
      unsubscribe = null;
    }
    previous = timer.timeElapsed;
    started = true;
    running = false;
    updateTimerMutation({id: timer.id, title: title, timeElapsed: timer.timeElapsed});
  }

</script>

<div class="stack-small">
  {#if editing}
    <form on:submit|preventDefault={onSave} class="stack-small" on:keydown={e => e.key === 'Escape' && onCancelEdit()}>
      <div class="form-group">
        <label for="timer-{timer.id}" class="timer-label"> New title for '{timer.title}'</label>
        <input bind:value={title} type="text" id="timer-{timer.id}" autoComplete="off" class="timer-text"/>
      </div>
      <div class="btn-group">
        <button class="btn timer-cancel" on:click={onCancelEdit} type="button">
          Cancel
        </button>
        <button class="btn btn__primary timer-edit" type="submit" disabled={!title}>
          Save
        </button>
      </div>
    </form>
  {:else}
    <label for="timer-{timer.id}" class="timer-label">{timer.title}</label>
    <div class="form-group">
      <p>
        {formatTime(timer.timeElapsed)}
      </p>
      <TimerControls on:startTiming={startTiming} on:pauseTiming={pauseTiming} {started} {running} />
    </div>
    <div class="btn-group">
      <button type="button" class="btn" on:click={onEdit}>
        Edit
      </button>
      <button type="button" class="btn btn__danger" on:click={onDelete}>
        Delete
      </button>
    </div>
  {/if}
</div>