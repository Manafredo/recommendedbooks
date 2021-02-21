<script>
    import {isActive, url} from '@roxi/routify'
  
    const links =
    [
      ["http://localhost:5000/index", 'Personen'],
      ["http://localhost:5000/books", 'Bücher'],
      ["http://localhost:5000/impressum", 'Impressum'],
      ["http://localhost:5000/kontakt", 'Kontakt']
    ]
  </script>
  <ul>
    {#each links as [path, name]}
      <!-- Svelte magic. If isActive is true, the "active" class is applied. -->
      <li class:active={$isActive(path)}>
        <a href={path}>
          {name}
        </a>
      </li>
    {/each}
  </ul>