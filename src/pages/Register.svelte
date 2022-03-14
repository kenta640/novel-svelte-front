<script>
    import {mutation} from "svelte-apollo"
    import {registerMutation} from "../schema.graphql"
    import {userData} from "../components/stores"
    import { useNavigate, useLocation } from "svelte-navigator";
    const register = mutation(registerMutation, {variables:{  "user": {
    "username": null,
    "password": null,
    "email": null
  }}})


    const navigate = useNavigate();
    const location = useLocation();
    //const navigate = useNavigate();
    let userName
    let userEmail
    let userPassword
    let confirmPassword
    let userToken


    const onSubmit = async (e) => {
  

  //handleLogin(userEmail, userPassword);
    const registerDetail = await register({variables:{ "user": {username: userName, email: userEmail, password: userPassword}}})

    userToken = registerDetail.data.RegisterUser.token
    console.log(userName)
    $userData = { userToken, userName, userEmail, userPassword, confirmPassword}

    if ($userData.userToken) {
        const from = ($location.state && $location.state.from) || "/";
        navigate(from, { replace: true });
    }
    

    
}
</script>

<main>
    <div class = "grid grid-cols-4 gap-4 h-70v">
    <div></div>
    <div>
    <form on:submit|preventDefault={onSubmit}>

        <div>
            <label for="name">userName</label>
            <input
              type="username"
              id="username"
              name="username"
              class="w-full px-4 py-2 mt-2 border rounded-md focus:outline-none focus:ring-1 focus:ring-blue-600"
              bind:value= {userName}
            />
        </div>
        <div>
            <label for="name">Email</label>
            <input
            type="email"
            id="email"
            name="email"
            class="w-full px-4 py-2 mt-2 border rounded-md focus:outline-none focus:ring-1 focus:ring-blue-600"
            bind:value= {userEmail}
            />
        </div>
        <div>
        <label for="name">Password</label>
        <input
            type="password"
            id="password"
            name="password"
            class="w-full px-4 py-2 mt-2 border rounded-md focus:outline-none focus:ring-1 focus:ring-blue-600"
            bind:value={userPassword}
        />
        <div>
            <label for="name"> Confirm Password</label>
            <input
            type="password"
            id="password"
            name="password"
            class="w-full px-4 py-2 mt-2 border rounded-md focus:outline-none focus:ring-1 focus:ring-blue-600"
            bind:value={confirmPassword}
            />
        <button type="submit" class="px-6 py-2 mt-4 text-white bg-blue-600 rounded-lg hover:bg-blue-900" >
            Register!
        </button>
    </form>
    </div>
    <div>

    </div>
</div>
</main>