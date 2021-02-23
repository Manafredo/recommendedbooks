<script>
    import Navigation from "../_Components/Navigation.svelte";
    import {onMount} from "svelte";
    import { params } from "@roxi/routify";

    let book =[];
    let description;

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
      console.log(book);
      
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
  <p>{description}</p> 
</main>
