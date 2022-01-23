<script>
    import { Router, Route, Link } from "svelte-navigator";
    import Login from "./pages/Login.svelte";
    import PrivateRoute from "./components/PrivateRoute.svelte";
    import Posts from "./pages/Posts.svelte"
    import Profile from "./pages/Profile.svelte"
    import Register from "./pages/Register.svelte"
    import { InMemoryCache, ApolloClient } from '@apollo/client'; 
    import { mutation, setClient } from "svelte-apollo";
   

    
    export const client = new ApolloClient({
        uri: 'https://js-graphql-server.herokuapp.com/graphql',
        cache: new InMemoryCache({
          //prevent duplicated results
          dataIdFromObject: o => (o._id ? `${o.__typename}:${o._id}`: null),
        })
    });
    setClient(client); 
  </script>
  
  <Router basepath="/">
    <header>
  
      <nav class="h-56 grid grid-cols-4 h-12">
        <button class="bg-orange-500 hover:bg-orange-600"><Link to="/">Home</Link></button>
        <button class="bg-orange-500 hover:bg-orange-600"><Link to="profile">Profile</Link></button>
        <button class="bg-orange-500 hover:bg-orange-600"><Link to="posts">Posts</Link></button>
        <button class="bg-orange-500 hover:bg-orange-600"><Link to="setting">Setting</Link></button>
      </nav>
    </header>
  
    <main>
    
      <Route path="login">
        <Login />
      </Route>
  
      <Route path="/">
        <h3>Home</h3>
        <p>Home sweet home...</p>
      </Route>
  
      <Route path="register" let:location>
        <h3>About</h3>
        <Register/>
      </Route>
  
      <PrivateRoute path="profile"  let:location> 
        <Profile/>
      </PrivateRoute>
      
      <Route path="posts" component={Posts}>
      </Route>
      
    </main>
    <footer> 
      <Link to="register">Register</Link>
    </footer>
  </Router>


  <style lang="postcss" global>
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
  </style>