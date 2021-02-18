<script>
    import Persons from "./_Components/Persons.svelte";
    import Navigation from "./_Components/Navigation.svelte";
    import { onMount } from "svelte";
  
    let persons = [];
  
    onMount(async function() {
      const response = await fetch(
        "https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?content_type=person&include=0&select=sys.id,fields&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
      );
      persons = await response.json();
      persons =  persons.items;
    });
  </script>

  <Navigation/>
  <Persons {persons}/>