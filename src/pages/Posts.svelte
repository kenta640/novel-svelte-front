<script>

import { mutation, query, subscribe} from "svelte-apollo";
import { postMutation, deletePostMutation } from '../schema.graphql';
import {userData} from "../components/stores"
import {topPostsQuery, addGoodMutation, postAddedSubscription} from "../schema.graphql"
import Modal from "../components/Modal.svelte"
import {modal} from "../components/stores"
import SubmitButton from "../components/SubmitButton.svelte"
import Icon from '@iconify/svelte';
import Popup from '../components/Popup.svelte';

      
      let opening = false;
      let opened = false;
      let closing = false;
      let closed = false;

    //Query posts by apollo query
    const topPosts = query(topPostsQuery)
    
    let textSubmitted;
    let postObjectId
    //Mutations
    const postAdded = subscribe(postAddedSubscription)
    const postText = mutation(postMutation)
    const addGood = mutation(addGoodMutation)
    const deletePost = mutation(deletePostMutation)
    //mutation
    const onSubmit = async (e) => {

        await postText({ variables: { addPostId: $userData.userId, text: textSubmitted} });
        textSubmitted=""
    }
    

    const onDelete =  async (e) => {
        
        await deletePost({variables: {postId: postObjectId, userId: $userData.userId}})
    }

    const showPopup = () => {
        console.log(userData.userName)
        if(!userData){
            open(Popup, { message: "Login or sign up!" });
        }    
    };
    let fake_posts=[
        { id: 'J---aiyznGQ', text: 'Goodmorning' },
		{ id: 'z_AbfPXTKms', text: 'Good Evening' },
		{ id: 'OUtn3pvWmpg', text: 'Good Aftenoon' },
        {id: 'OUtn3pvWmpg', text: 'Hello'}
    ]
</script>


<main>
    <div class = "grid grid-cols-4 gap-4 h-70v">
        <div></div>
        <div class = "overflow-x-clip overflow-y-auto col-span-2">
        
           
                {#each fake_posts as post}
                    <div class="box-border 
                    border-4 bg-gray-100 hover:bg-gray-200">
                        <div class="flex flex-row items-stretch">
                            <button class="text-green-600" type="button">
                                Kenta 
                            </button>
                            <button type="button">
                                <svg class="h-4 w-4 text-red-500"  
                                    viewBox="0 0 24 24"  
                                    fill="none"  
                                    stroke="currentColor"  
                                    stroke-width="2"  
                                    stroke-linecap="round"  
                                    stroke-linejoin="round">  
                                    <circle cx="12" cy="12" r="1" />  
                                    <circle cx="19" cy="12" r="1" />  
                                    <circle cx="5" cy="12" r="1" />
                                </svg>
                            </button>
                        </div>
                        <p class="break-words">
                            {post.text}
                        </p>
                        <div class="flex flex-row">
                            <button type="button" >
                                <svg 
                                    class="h-5 w-5 text-red-500"  
                                    viewBox="0 0 24 24"  
                                    fill="none"  
                                    stroke="currentColor"  
                                    stroke-width="2"  
                                    stroke-linecap="round"  
                                    stroke-linejoin="round">  
                                    <path d="M14 9V5a3 3 0 0 0-3-3l-4 9v11h11.28a2 2 0 0 0 2-1.7l1.38-9a2 2 0 0 0-2-2.3zM7 22H4a2 2 0 0 1-2-2v-7a2 2 0 0 1 2-2h3" />
                                </svg>
                            </button>
                            
                        </div>
                    </div>
                {/each}
           
        </div>
    </div>

    <textarea name="text" type="text" id="text" bind:value= {textSubmitted}
    class="w-full px-4 py-2 mt-2 border rounded-md 
    focus:outline-none focus:ring-1 focus:ring-blue-600"/>

    <Modal show={$modal}>
        <button on:click={showPopup} type="submit" 
            class="px-6 py-2 mt-4 text-white bg-blue-600 rounded-lg 
            hover:bg-blue-900">Submit!
        </button>
            <div id="state">
                {#if opening}
                    <p>opening modal...</p>
                {:else if opened}
                    <p>opened modal!</p>
                {:else if closing}
                    <p>closing modal...</p>
                {:else if closed}
                    <p>closed modal!</p>
                {/if}
            </div>
      <br/>
    </Modal>
    <br/>
    <form on:submit|preventDefault={onSubmit}>
        
        <div class = "grid grid-cols-4 gap-4 h-70v">
            <div></div>
            <div class = "overflow-x-clip overflow-y-auto col-span-2">
            
                {#if $topPosts.loading}
                    <div class="
                    spinner-border
                    animate-spin
                    inline-block
                    w-8
                    h-8
                    border-4
                    rounded-full
                    text-blue-500
                " role="status">
                    <span class="visually-hidden">Loading...</span>
                </div>
                {:else if $topPosts.error}
                <p>Error: {$topPosts.error.message}</p>
                {:else if $topPosts.data}
                    {#each $topPosts.data.topPosts as post}
                        <div class="box-border 
                        border-4 bg-gray-100 hover:bg-gray-200">
                            <div class="flex flex-row items-stretch">
                                <button class="text-green-600" type="button">
                                    {post.user.username} 
                                </button>
                                <button type="button">
                                    <svg class="h-4 w-4 text-red-500"  
                                        viewBox="0 0 24 24"  
                                        fill="none"  
                                        stroke="currentColor"  
                                        stroke-width="2"  
                                        stroke-linecap="round"  
                                        stroke-linejoin="round">  
                                        <circle cx="12" cy="12" r="1" />  
                                        <circle cx="19" cy="12" r="1" />  
                                        <circle cx="5" cy="12" r="1" />
                                    </svg>
                                </button>
                            </div>
                            <p class="break-words">
                                {post.text}
                            </p>
                            <div class="flex flex-row">
                                <button type="button" 
                                on:click={async (e) => {
                                    await addGood({ variables:{postid: post._id}})
                                }}>
                                    <svg 
                                        class="h-5 w-5 text-red-500"  
                                        viewBox="0 0 24 24"  
                                        fill="none"  
                                        stroke="currentColor"  
                                        stroke-width="2"  
                                        stroke-linecap="round"  
                                        stroke-linejoin="round">  
                                        <path d="M14 9V5a3 3 0 0 0-3-3l-4 9v11h11.28a2 2 0 0 0 2-1.7l1.38-9a2 2 0 0 0-2-2.3zM7 22H4a2 2 0 0 1-2-2v-7a2 2 0 0 1 2-2h3" />
                                    </svg>
                                </button>
                                {post.good.good}
                            </div>
                        </div>
                    {/each}
                {/if}
            </div>
        </div>

        <textarea name="text" type="text" id="text" bind:value= {textSubmitted}
        class="w-full px-4 py-2 mt-2 border rounded-md 
        focus:outline-none focus:ring-1 focus:ring-blue-600"/>
    
        <Modal show={$modal}>
            <button on:click={showPopup} type="submit" 
                class="px-6 py-2 mt-4 text-white bg-blue-600 rounded-lg 
                hover:bg-blue-900">Submit!
            </button>
                <div id="state">
                    {#if opening}
                        <p>opening modal...</p>
                    {:else if opened}
                        <p>opened modal!</p>
                    {:else if closing}
                        <p>closing modal...</p>
                    {:else if closed}
                        <p>closed modal!</p>
                    {/if}
                </div>
          <br/>
        </Modal>
        <br/>

    </form>

</main>


