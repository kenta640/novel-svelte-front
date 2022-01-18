
<script>
    import { query } from "svelte-apollo";
    import { postsByIdQuery } from '../schema.graphql';
    import { userData } from "../components/stores";
    function handleLogout() {
      $userData = null;
    }

    const myPosts = query(postsByIdQuery, {variables:{postId: $userData.userId}})
    
</script>

<main>

    <h3>Welcome {$userData.userName}</h3>

    <h4>My posts</h4>

    {#if $myPosts.loading}
    <p>Loading...</p>
    {:else if $myPosts.error}
    <p>Error: {$myPosts.error.message}</p>
    {:else}
        {#each $myPosts.data.postsById as post}
        <div class="box-border 
                    border-4 bg-gray-100 m4 hover:bg-gray-200">
            <p>{post.text }</p><br/>
        </div>
        {/each}
    {/if}




    <button on:click={handleLogout} 
    class="px-6 py-2 mt-4 text-white bg-gray-400 rounded-lg hover:bg-gray-900">
    Logout</button>
</main>
