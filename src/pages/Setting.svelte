<script>
    import { Router, Route, Link } from "svelte-navigator";
  

  </script>
  
  <div class="Blog">
    <h2>Blog</h2>

  
    <nav>
      <Link to="../">Go back to the landing page</Link>
    </nav>
  
    <p>
      Read about interesting stuff here, like
      <Link to="svelte">Svelte</Link>
      or
      <Link to="navigator">Svlete Navigator</Link>
    </p>
  
    <Router>
      <article>
  
        <Route path="svelte">
          <h2>Svelte</h2>
          <p>
            Svelte is a radical new approach to building user interfaces. Whereas
            traditional frameworks like React and Vue do the bulk of their work in
            the browser, Svelte shifts that work into a compile step that happens
            when you build your app. Instead of using techniques like virtual DOM
            diffing, Svelte writes code that surgically updates the DOM when the
            state of your app changes.
            <a href="https://svelte.dev/blog/svelte-3-rethinking-reactivity">
              Read the introductory post
            </a>
            to learn more.
          </p>
        </Route>
  
        <Route path="navigator">
          <h2>Navigator</h2>
          <p>
            Simple, declarative routing for single page apps built with Svelte.
          </p>
        </Route>
      </article>
    </Router>
  </div>