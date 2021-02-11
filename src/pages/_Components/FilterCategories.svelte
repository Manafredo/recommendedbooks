<script>
    export let filter = "Alle";
  </script>
  
  <div class="filterCategories btn-group stack-exception">
    <button class="btn toggle-btn" class:btn__primary={filter === "Politiker"} aria-pressed={filter === "Politiker"} on:click={()=> filter = "Politiker"} >
      <span>Politiker</span>
    </button>
    <button class="btn toggle-btn" class:btn__primary={filter === "Entrepreneur"} aria-pressed={filter === "Entrepreneur"} on:click={()=> filter = "Entrepreneur"} >
      <span>Entrepreneur</span>
    </button>
    <button class="btn toggle-btn" class:btn__primary={filter === 'Alle'} aria-pressed={filter === 'Alle'} on:click={()=> filter = 'Alle'} >
      <span>Alle</span>
    </button>
  </div>