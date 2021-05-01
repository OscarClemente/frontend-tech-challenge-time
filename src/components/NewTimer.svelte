<script>
  import {createEventDispatcher} from 'svelte';
  import {mutation} from '@urql/svelte';

  const dispatch = createEventDispatcher()

  const newTimerMutation = mutation({
    query: `
      mutation($title: String!) {
        createTimer(title: $title) {
          id,
          title,
          timeElapsed,
          createdAt,
        }
      }`
  });

  let title = ''

  const addTimer = () => {
    newTimerMutation({title: title}).then(result => {
      var createdAt = new Date(result.data.createTimer.createdAt);
      dispatch('addTimer', {id: result.data.createTimer.id,
                           title: result.data.createTimer.title,
                           timeElapsed: result.data.createTimer.timeElapsed,
                           createdAt: createdAt});
      title = '';
    });
  }

  const onCancel = () => {
    title = '';
  }

</script>

<form on:submit|preventDefault={addTimer} on:keydown={e => e.key === 'Escape' && onCancel()}>
  <h2 class="label-wrapper">
    <label for="timer-0" class="label__lg">What's your next task?</label>
  </h2>
  <input bind:value={title} type="text" id="timer-0" autoComplete="off" class="input input__lg"/>
  <button type="submit" disabled={!title} class="btn btn__primary btn__lg">Add</button>
</form>