
<script>
    import MeetupItem from './MeetupItem.svelte';
    import MeetupFilter from './MeetupFilter.svelte';
    import Button from '../UI/Button.svelte';
    import {createEventDispatcher} from 'svelte';
    import {flip} from 'svelte/animate';
    
    import {scale} from 'svelte/transition';
    const dispatch = createEventDispatcher();
    export let meetups;
    let favsOnly = false;
    $: filterMeetups = favsOnly ? meetups.filter(m => m.isFavorite ): meetups;
    function setFilter(event){
        favsOnly = event.detail === 1;
    }
</script>
<style>
    section{
        width: 100%;
        display: grid;
        grid-template-columns:1fr;
        grid-gap: 1rem;
    }
    #meetup-controls{
        margin: 1rem;
    }
    @media (min-width:768px){
        section{
            grid-template-columns: repeat(2,1fr);
        }
    }
</style>
<section id="meetup-controls">
    <MeetupFilter on:select="{setFilter}"/>
    <Button on:click="{()=>dispatch('add')}" >New meetup</Button>
</section>
<section id="meetups">
	{#each filterMeetups as meetup (meetup.id)}
        <div animate:flip={{duration:300}} transition:scale>
		<MeetupItem 
            id={meetup.id}
            title={meetup.title} 
            subtitle={meetup.subtitle} 
            imageUrl={meetup.imageUrl} 
            description={meetup.description} 
            address={meetup.address} 
            email={meetup.contactEmail}
            isFav={meetup.isFavorite}
            on:showdetails
            on:edit
            />
        </div>
	{/each}
</section>
