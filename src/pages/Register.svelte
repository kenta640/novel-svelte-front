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
    console.log(registerDetail.data.RegisterUser.token)
    $userData = { userToken, userName, userEmail, userPassword, confirmPassword}

    if ($userData.userToken) {
        const from = ($location.state && $location.state.from) || "/";
        navigate(from, { replace: true });
    }
    
    
}
</script>

<main>
    <form on:submit|preventDefault={onSubmit}>
        <div>
            <label for="name">userName</label>
            <input
              type="username"
              id="username"
              name="username"
              bind:value= {userName}
            />
        </div>
        <div>
            <label for="name">Email</label>
            <input
            type="email"
            id="email"
            name="email"
            bind:value= {userEmail}
            />
        </div>
        <div>
        <label for="name">Password</label>
        <input
            type="password"
            id="password"
            name="password"
            bind:value={userPassword}
        />
        <div>
            <label for="name">Password</label>
            <input
            type="password"
            id="password"
            name="password"
            bind:value={confirmPassword}
            />
        <button type="submit" >Register!</button>
    </form>

</main>