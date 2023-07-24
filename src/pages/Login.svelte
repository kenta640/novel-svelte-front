<script>
  import { InMemoryCache, ApolloClient } from '@apollo/client'; // ❶
  import { mutation } from "svelte-apollo"; // ❸
  import { loginMutation } from '../schema.graphql'; // ❷
  import {userData} from "../components/stores"
  import { useNavigate, useLocation } from "svelte-navigator";

  //Apollo client configuration
  
  const navigate = useNavigate();
  const location = useLocation();
  
  let userEmail
  let userPassword
  let userName
  let userToken
  let userId
    // addTask
    
    const login = mutation(loginMutation);
  
  
    const onSubmit = async (e) => {
    
      //handleLogin(userEmail, userPassword);
      const userDetail = await login({ variables: { email: userEmail, password: userPassword } });    
      if(userEmail=="user@example.com"||password=="user"){

      }
      
      userName = userDetail.data.LoginUser.username
      userToken = userDetail.data.LoginUser.token
      userId = userDetail.data.LoginUser.id
      
      $userData = { userId, userToken, userName: userName, userEmail, userPassword };
      
      //User will be directed only they have their username in $user cache
      if ($userData.userToken) {
        const from = ($location.state && $location.state.from) || "/";
        navigate(from, { replace: true });
      }
    }
  
  </script>
  
  <div>
  
    <form on:submit|preventDefault={onSubmit}>
      <div>
          <label for="name">Email</label>
          <input
            type="email"
            id="email"
            name="email"
            bind:value= {userEmail}
            class="w-full px-4 py-2 mt-2 border rounded-md focus:outline-none focus:ring-1 focus:ring-blue-600"
          />
      </div>
      <div>
        <label for="name">Password</label>
        <input
          type="password"
          id="password"
          name="password"
          bind:value={userPassword}
          class="w-full px-4 py-2 mt-2 border rounded-md focus:outline-none focus:ring-1 focus:ring-blue-600"
        />
    </div>
      <button type="submit" class="px-6 py-2 mt-4 text-white bg-blue-600 rounded-lg hover:bg-blue-900">
        Login!
      </button>  
      
      <br/>
      Test Account:
      <br/>
      email "user@example.com"
      <br/>
      password "user"
  
    </form>

 
    
  
  </div>