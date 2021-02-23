<script>
    import Navigation from "../_Components/Navigation.svelte";
    import {onMount} from "svelte";
    import { params } from "@roxi/routify";

    let book =[];
    let persons = [];
    let personsarr =[];
    let commentsarr =[];
    let comments =[];
    let personsWithComments =[];
    let description;
    let author;

    let title = $params.buch;
    title = title.replace("_"," ");
    title = title.replace("%20"," ");
    title = title.replace("%20"," ");
    title = title.replace("%20"," ");
    title = title.replace("%20"," ");
    title = title.replace("%20"," ");
    title = title.replace("%20"," ");


    onMount(async function() {
      const response = await fetch("https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?select=sys.id,fields&content_type=book&include=0&fields.title="+title+"&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
      );
      book = await response.json();
      book= book.items;
      description = book["0"]["fields"]["description"];
      author = book["0"]["fields"]["author"];
      
      const response3 = await fetch("https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?content_type=personBook&include=2&fields.book.sys.id="+book["0"]["sys"]["id"]+"&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
      );
      let comments = await response3.json();
      persons = comments.includes.Entry;
      comments=comments.items;
      for (let i=0; i<comments.length; i++){
        let obj = comments[i];
        commentsarr.push({id: obj.fields.person.sys.id, sourcedescription: obj.fields.sourcedescription})
      }
      for (let i=0; i<persons.length; i++){
        let obj = persons[i];
        personsarr.push({id: obj.sys.id, name: obj.fields.name, description: obj.fields.description})
      }
      personsWithComments=commentsarr.map(t1 => ({...t1, ...personsarr.find(t2=>t2.id===t1.id)}));
    });
</script>

<link rel="stylesheet" href="https://use.typekit.net/cal1lzu.css">
<style>
  h2,p {
    font-family:"myriad-pro";
  }
</style>
<Navigation/>
<main>
  <h2>{title}</h2>
  <p> {author}</p>
  <p>{description}</p>
  <ul>
    {#each personsWithComments as person}
        <p>{person.name}</p>
        <p>Source: {person.sourcedescription}</p>
    {:else}
    <!-- this block renders when book.length === 0 -->
    <p>loading...</p>
    {/each}
  </ul>
</main>
