<script>
    import { query } from "svelte-apollo"
    import {userData} from "../components/stores"
    import {postByObjIdQuery} from "../schema.graphql"
    
    let postObjId 

    const postByObjId = query(postByObjIdQuery)
    const post = postByObjId({variables: {_id: postByObjId}})
</script>


<div>{post.data.postByObjId.text}</div>

