<script>
import { mutation, query } from "svelte-apollo";
import { postMutation } from '../schema.graphql';
import {userData} from "../components/stores"
import {allPostsQuery} from "../schema.graphql"
import Modal from "../components/Modal.svelte"
import {modal} from "../components/stores"
import SubmitButton from "../components/SubmitButton.svelte"
import Popup from '../components/Popup.svelte';
import { getContext } from 'svelte';
    //Query posts by apollo query
    const allPosts = query(allPostsQuery)
    
    let textSubmitted;

    //Mutation
    const postText = mutation(postMutation)

    //mutation
    const onSubmit = async (e) => {

        await postText({ variables: { addPostId: $userData.userId, text: textSubmitted} });
        textSubmitted=""
    }


</script>


<main>
    
    <form on:submit|preventDefault={onSubmit}>
        <h3>Posts</h3>
        <div >
        
            {#if $allPosts.loading}
            <p>Loading...</p>
            {:else if $allPosts.error}
            <p>Error: {$allPosts.error.message}</p>
            {:else}
                {#each $allPosts.data.allPosts as post}
                    <div class="box-border 
                    border-4 bg-gray-100 m4 hover:bg-gray-200">
                        <p class="text-green-600">
                            {post.user.username} 
                        </p>
                            
                        <p>
                            {post.text}
                        </p>
                    </div>
                {/each}
            {/if}
        
        </div>
        <div>
            <textarea name="text" type="text" id="text" bind:value= {textSubmitted}
            class="w-full px-4 py-2 mt-2 border rounded-md focus:outline-none focus:ring-1 focus:ring-blue-600"/>
        </div>
        <Modal show={$modal}>
            <SubmitButton />
        </Modal>
        <br/>

    </form>

</main>