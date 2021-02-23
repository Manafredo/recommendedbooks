<script>
    import Navigation from "../_Components/Navigation.svelte";
    import {onMount} from "svelte";
    import { params, goto } from "@roxi/routify";

    let books =[];
    let person = [];
    let bookarr =[];
    let commentarr=[];
    let booksWithComments=[];

    let name;
    let description;

    let personName = $params.person;
    personName = personName.replace("_"," ");

    onMount(async function() {
      const response = await fetch("https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?select=sys.id,fields&content_type=person&include=0&fields.name="+personName+"&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
      );
      person = await response.json();
      person= person.items;
      name=person["0"]["fields"]["name"];
      description = person["0"]["fields"]["description"];
      //console.log(person);
      
      const response2 = await fetch("https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?content_type=book&include=2&fields.person.sys.id="+person["0"]["sys"]["id"]+"&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
      );
      books = await response2.json();
      books=books.items;
      for (let i=0; i<books.length; i++){
        let obj = books[i];
        bookarr.push({id: obj.sys.id, title: obj.fields.title, description: obj.fields.description})
      }
      //console.log(bookarr);

      const response3 = await fetch("https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?content_type=personBook&include=2&fields.person.sys.id="+person["0"]["sys"]["id"]+"&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
      );
      let comments = await response3.json();
      comments=comments.items;
      for (let i=0; i<comments.length; i++){
        let obj = comments[i];
        commentarr.push({id: obj.fields.book.sys.id, sourcedescription: obj.fields.sourcedescription})
      }
      //console.log(commentarr);
      booksWithComments=bookarr.map(t1 => ({...t1, ...commentarr.find(t2=>t2.id===t1.id)}));
      //console.log(booksWithComments);

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
  <h2>{name}</h2>
  <p>{description}</p> 
    <ul>
      {#each booksWithComments as book}
          <a href={"http://localhost:5000/buch/" + book.title}>{book.title}</a>
          <p>{book.description}</p>
          <p>Source: {book.sourcedescription}</p>
      {:else}
      <!-- this block renders when book.length === 0 -->
      <p>loading...</p>
      {/each}
    </ul>
  </main>
