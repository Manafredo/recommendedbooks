<script>
    import FilterCategories from "./FilterCategories.svelte";
    import {url} from '@roxi/routify'

    export let persons = [];
  
    let filter = "all";
    const filterPersons = (filter, persons) =>
      filter === "Politiker"
        ? persons.filter(p => p.fields.category == "Politiker")
        : filter === "Entrepreneur"
        ? persons.filter(p => p.fields.category == "Entrepreneur")
        : persons;
  </script>
  
  <link rel="stylesheet" href="https://use.typekit.net/cal1lzu.css">
  <style>
    p,a {
      font-family:"myriad-pro";
    }
  </style>

  <FilterCategories bind:filter/>
    <ul>
      {#each filterPersons(filter,persons) as person}
          <a href={$url("person/"+ person["fields"]["name"])}>{person["fields"]["name"]}</a>
          <p>{person["fields"]["description"]}</p>
      {/each}
    </ul>
