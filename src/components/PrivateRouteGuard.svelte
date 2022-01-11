<script>
    import { useNavigate, useLocation } from "svelte-navigator";
    import { userData } from "./stores";
  
    const navigate = useNavigate();
    const location = useLocation();
  
    $: if (!$userData) {
      navigate("/login", {
        state: { from: $location.pathname },
        replace: true,
      });
    }
  </script>
  
  {#if $userData}
    <slot />
  {/if}