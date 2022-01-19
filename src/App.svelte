<script>
    import { Router, Route, Link } from "svelte-navigator";
    import Login from "./pages/Login.svelte";
    import PrivateRoute from "./components/PrivateRoute.svelte";
    import Posts from "./pages/Posts.svelte"
    import Profile from "./pages/Profile.svelte"
    import Register from "./pages/Register.svelte"
    import Setting from "./pages/Setting.svelte"
    import { InMemoryCache, ApolloClient } from '@apollo/client'; 
    import { mutation, setClient } from "svelte-apollo";
    
    const httpLink = new HttpLink({
      uri: 'https://js-graphql-server.herokuapp.com/graphql'
    });
    const wsLink = new WebSocketLink({
      uri: `https://js-graphql-server.herokuapp.com/subscriptions`,
      options: {
        reconnect: true
      }
    });


    const link = split(
      // split based on operation type
      ({ query }) => {
        const definition = getMainDefinition(query);
        return (
          definition.kind === 'OperationDefinition' &&
          definition.operation === 'subscription'
        );
      },
      wsLink,
      httpLink,
    );
    
    
    export const client = new ApolloClient({
        link,
        cache: new InMemoryCache({
          //prevent duplicated results
          dataIdFromObject: o => (o._id ? `${o.__typename}:${o._id}`: null),
        })
    });
    setClient(client); 
  </script>
  
  <Router basepath="/">
    <header>
  
      <nav class=" bg-gradient-to-r from-pink-500 to-violet-500">
        <Link to="/">Home</Link>
        
        <Link to="profile">Profile</Link>
        <Link to="posts">Posts</Link>
        <Link to="setting">Setting</Link>
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