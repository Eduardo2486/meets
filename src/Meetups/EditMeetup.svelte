<script>
    import { createEventDispatcher } from 'svelte';
    import TextInput from '../UI/TextInput.svelte';
    import Button from '../UI/Button.svelte';
    import Modal from '../UI/Modal.svelte';
    import {isEmpty} from '../helpers/validation.js';
    import meetups from './meetups-store.js';

    export let id = null;

    

    let title = '';
	let subtitle = '';
	let description = '';
	let imageUrl = '';
	let address = '';
    let contactEmail = '';
    let formIsValid = false;

    
    if(id){
        const unsubscribe = meetups.subscribe(items => {
            const selectedMeetup = items.find(item => item.id === id);
            title = selectedMeetup.title;
            subtitle = selectedMeetup.subtitle;
            description = selectedMeetup.description;
            imageUrl = selectedMeetup.imageUrl;
            address = selectedMeetup.address;
            contactEmail = selectedMeetup.contactEmail;
        });
        unsubscribe();
    }
    
    $: titleValid = !isEmpty(title);
    $: subtitleValid = !isEmpty(subtitle);
    $: addressValid = !isEmpty(address);
    $: DescriptionValid = !isEmpty(description);
    $: contactEmailValid = !isEmpty(contactEmailValid);
    $: imageUrlValid = !isEmpty(imageUrl);
    $: formIsValid = titleValid&&subtitleValid&&addressValid&&DescriptionValid&&contactEmailValid&&imageUrlValid;

    const dispatch = createEventDispatcher();
    function submitForm(){

        const meeetupData = {
			id: Math.random().toString(),
			title: title,
			subtitle: subtitle,
			description: description,
			imageUrl: imageUrl,
			address: address,
			contactEmai: contactEmail
        };
        
        if(id){
            fetch("http://localhost:3000/home/meetups",{
                method:"PATCH",
                body: JSON.stringify({meeetupData,id}),
                headers:{"Content-Type":"application/json"}
            })
            .then(res => {
                if(!res.ok){
                    throw new Error("Dont updated");
                }
                meetups.updateMeetup(id, meeetupData);
            })
            .catch(err=>{
                console.log(err)
            });
        }else{
            fetch("http://localhost:3000/home/meetups", {
                method:"POST",
                body:JSON.stringify({...meeetupData,isFavorite:false}),
                headers:{'Content-Type':'application/json'}
            })
            .then(res => {
                if(!res.ok){
                    throw new Error("Failed");
                }
                return res.json();
            })
            .then(data =>{
                console.log(data);
                meetups.addMeetup({...meeetupData,isFavorite:false,id:data.id});
            })
            .catch(err=>{
                console.log(err);
            });
        }
		
        dispatch('save');
    }
    function deleteMeetup(){
        meetups.removeMeetup(id);
        dispatch("save");
    }
    function cancel(){
        dispatch('cancel');
    }
</script>
<style>
form{
		width:100%;
	}
</style>
<Modal title="Edit Meetup data" on:cancel>
    <form on:submit|preventDefault="{submitForm}">
        <TextInput 
            id="title" 
            label="Title" 
            value={title} 
            validityMessage="Please enter a valid title"
            valid={titleValid}
            on:input={(event=> title = event.target.value)} 
            type='text'/>
        <TextInput 
            id="subtitle" 
            label="Subtitle" 
            value={subtitle} 
            validityMessage="Please enter a valid subtitle"
            valid={subtitle}
            on:input={(event=> subtitle = event.target.value)} 
            type='text'/>
        <TextInput 
            id="address" 
            label="Address" 
            value={address} 
            validityMessage="Please enter a valid address"
            valid={addressValid}
            on:input={(event=> address = event.target.value)} 
            type='text'/>
        <TextInput 
            id="imageUrl" 
            label="Image URL" 
            value={imageUrl} 
            validityMessage="Please enter a valid image"
            valid={imageUrlValid}
            on:input={(event=> imageUrl = event.target.value)} 
            type='text'/>
        <TextInput 
            id="email" 
            label="Email" 
            value={contactEmail} 
            validityMessage="Please enter a valid email"
            valid={contactEmailValid}
            on:input={(event=> contactEmail = event.target.value)} 
            type='text'/>
        <TextInput 
            id="description" 
            label="Description" 
            value={description}
            on:input={(event=> description = event.target.value)} 
            controlType='textarea' rows='4' />
        
    </form>
                                                
    <div slot="footer">
        <Button type="submit" mode="outline" on:click={submitForm} disabled={!formIsValid}>Save</Button>
        <Button type="submit" on:click={cancel}>Cancel</Button>
        {#if id}
            <Button type="button" on:click={deleteMeetup}>Delete</Button>
        {/if}
    </div>
</Modal>