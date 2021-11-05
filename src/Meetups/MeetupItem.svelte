<script>
    import Button from '../UI/Button.svelte';
    import Badge from '../UI/Badge.svelte';
    import meetups from './meetups-store.js';
    import {createEventDispatcher} from 'svelte';
    const dispatch = createEventDispatcher();
    export let id;
    export let title;
    export let subtitle;
    export let imageUrl;
    export let description;
    export let address;
    export let email;
    export let isFav;

    function togglefavorite(){
      meetups.toggleFavorite(id);
    }
</script>

<style>
 article {
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
    border-radius: 5px;
    background: white;
    margin: 1rem;
  }

  header,
  .content,
  footer {
    padding: 1rem;
  }

  .image {
    width: 100%;
    height: 14rem;
  }

  .image img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  h1 {
    font-size: 1.25rem;
    margin: 0.5rem 0;
    font-family: "Roboto Slab", sans-serif;
  }

  h1.is-favorite {
    background: #01a129;
    color: white;
    padding: 0 0.5rem;
    border-radius: 5px;
  }

  h2 {
    font-size: 1rem;
    color: #808080;
    margin: 0.5rem 0;
  }

  p {
    font-size: 1.25rem;
    margin: 0;
  }

  div {
    text-align: right;
  }

  .content {
    height: 4rem;
  }
</style>

<article  >
    <header>
        <h1>{title} 
          {#if isFav}
            <Badge>FAVORITE</Badge>
          {/if}
        </h1>    
        <h2>{subtitle}</h2>
        <p>{address}</p>
    </header>
    <div class="image">
        <img src="{imageUrl}" alt="">
    </div>
    <div class="content">
        <p>{description}</p>
    </div>
    <svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" preserveAspectRatio="xMidYMid meet" viewBox="0 0 62 82.67" width="62" height="82.67">
                                                    <path fill="white" d="M0 82.21L31 41.34L0 0.46L0 82.21Z"></path>
                                                    <path fill="white" d="M62 0L31 41.34L0 0L62 0Z"></path>
                                                    <path fill="white" d="M62 82.67L31 40.74L62 0L62 82.67Z"></path>
                                                    <path fill="white" d="M0 82.67L31 41.34L62 82.67L0 82.67Z"></path>
                                                    <path d="M0 0L0.57 0L31 40.41L61.38 0L62 0L62 0.68L31.73 41.34L62 81.7L62 82.67L61.32 82.67L31 42.14L0.49 82.67L0 82.67L0.3 81L30.33 41.34L0 1.73L0 0Z"></path>
                                                </svg>
    <footer>
        <Button mode="outline" type="button" on:click={()=>{dispatch('edit', id)}} >Edit</Button>
        <Button type="button" on:click="{()=>{dispatch('showdetails', id)}}">Show details</Button>
        <Button 
          mode="outline"
          color={isFav ? null : "success"} 
          type="button" 
          on:click="{togglefavorite}">{isFav ? "Unfavorite" : "Favorite"}</Button>
    </footer>
</article>