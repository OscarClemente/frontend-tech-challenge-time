<script>
  export let timer;
  let editing = false;

  function onEdit() {
    editing = true;
  }

  function onDelete() {
    console.log("delete");
  }
</script>

<div class="stack-small">
  <label for="timer-{timer.id}" class="timer-label">{timer.title}</label>
  <div class="form-group">
    {timer.timeElapsed}
  </div>
  <div class="btn-group">
    <button type="button" class="btn" on:click={onEdit}>
      Edit
    </button>
    <button type="button" class="btn btn__danger" on:click={onDelete}>
      Delete
    </button>
  </div>
</div>