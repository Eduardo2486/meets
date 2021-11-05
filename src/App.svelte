<script>
	import Header from './UI/Header.svelte';
	import MeetupGrid from './Meetups/MeetupGrid.svelte';
	import TextInput from './UI/TextInput.svelte';
	import Button from './UI/Button.svelte';
	import EditMeetup from './Meetups/EditMeetup.svelte';
	import meetups from './Meetups/meetups-store.js';
	import MeetupDetail from './Meetups/MeetupDetail.svelte'
	
	let editMode = undefined;
	let page = "overview";
	let pageData = {};
	let editedId = null;
	let isLoading = true;
	// let meetups = 

	fetch('http://localhost:3000/home/meetups')
		.then(res => {
			isLoading= true;
			if(!res.ok){
				throw new Error("Error, data not fetched");
			}
			return res.json();
		})
		.then( data => {
			let loadedMeetups = [];
			for(const key in data){
				loadedMeetups.push({
					...data[key],
					id:data[key]._id
				});
			}
			meetups.setMeetups(loadedMeetups);
			isLoading=false
		})
		.catch(err=>{
			console.log(err);
			isLoading=false;
		});
	
	function savedMeetup(event){
		editMode=null;
		editedId=null;
	}

	function cancelEdit(){
		editMode=null;
		editedId=null;
	}
	function showDetails(event){
		page = 'details';
		pageData.id = event.detail;
	}
	function closeDetails(){
		page = 'overview';
		pageData = {};
	}
	function startEdit(event){
		editMode ='edit'; 
		editedId = event.detail;
	}
</script>
<style>
	main{
		margin-top: 5rem;
	}
	.meetup-controls{
		margin:1rem;
	}
</style>
<Header/>
<main>
	{#if page === 'overview'}
				
		{#if editMode == 'edit'}
			<EditMeetup id={editedId} on:save="{savedMeetup}" on:cancel="{cancelEdit}"/>
		{/if}
		{#if isLoading}
			<h1>LOADING</h1>
		{:else}
			<MeetupGrid meetups={$meetups} on:showdetails={showDetails} on:edit={startEdit} on:add={()=> editMode = 'edit'}/>
		{/if}
	{:else}
		<MeetupDetail id={pageData.id} on:close="{closeDetails}"/>
	{/if}
</main>
