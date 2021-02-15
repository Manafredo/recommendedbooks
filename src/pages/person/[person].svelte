<script>
    import Navigation from "../_Components/Navigation.svelte";
    import {onMount} from "svelte";
    import { params } from "@roxi/routify";
import Booklist from "../_Components/booklist.svelte";

    let books =[];
    let person = [];
    let comment;

    let name;
    let description;
    let id;

    let personName = $params.person;
    personName = personName.replace("_"," ");

    onMount(async function() {
      const response = await fetch("https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?select=sys.id,fields&content_type=person&include=0&fields.name="+personName+"&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
      );
      person = await response.json();
      person= person.items;
      console.log(person);


      const response2 = await fetch("https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?content_type=personBook&include=2&fields.person.sys.id="+person["0"]["sys"]["id"]+"&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
      );
      let comments = await response2.json();
      comments=comments.items;
      console.log(comments);

      const response3 = await fetch("https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?content_type=book&include=2&fields.person.sys.id="+person["0"]["sys"]["id"]+"&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
      );
      books = await response3.json();
      books=books.items;
      console.log(books);

      name=person["0"]["fields"]["name"];
      description = person["0"]["fields"]["description"];
      comment = comments["0"]["fields"]["sourcedescription"];
      id= person["0"]["sys"]["id"];
    });
    
</script>

<Navigation/>
<Booklist id= "{id}"/>
<main>
  <h2>{name}</h2>
  <p>{description}</p> 
    <ul>
      {#each books as book}
          <h2>{book["fields"]["title"]}</h2>
          <p>{book["fields"]["description"]}</p>
          <p>Source: {comment}</p>
      {/each}
    </ul>
  </main>
