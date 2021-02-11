<script>
    import {isActive, url} from '@roxi/routify'
  
    const links =
    [
      ['./index', 'Home'], //add index if you don't want siblings to be considered children
      ['./Personen', 'Personen'],
      ['./Impressum', 'Impressum'],
      ['./contact', 'Contact']
    ]
  </script>
  
  <style>
    .active {font-weight: bold}
  </style>
  
  <ul>
    {#each links as [path, name]}
      <!-- Svelte magic. If isActive is true, the "active" class is applied. -->
      <li class:active={$isActive(path)}>
        <a href={$url(path)}>
          {name}
        </a>
      </li>
    {/each}
  </ul>