<script>
  export let filter = 'all'
</script>

<div class="filters btn-group stack-exception">
  <button class="btn toggle-btn" class:btn__primary={filter === 'all'} aria-pressed={filter === 'all'} on:click={()=> filter = 'all'} >
    All
  </button>
  <button class="btn toggle-btn" class:btn__primary={filter === 'today'} aria-pressed={filter === 'today'} on:click={()=> filter = 'today'} >
    Today
  </button>
  <button class="btn toggle-btn" class:btn__primary={filter === 'week'} aria-pressed={filter === 'week'} on:click={()=> filter = 'week'} >
    Week
  </button>
  <button class="btn toggle-btn" class:btn__primary={filter === 'month'} aria-pressed={filter === 'month'} on:click={()=> filter = 'month'} >
    Month
  </button>
</div>