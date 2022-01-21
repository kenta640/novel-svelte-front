<script>
    import { getContext } from 'svelte';
    import Popup from '../components/Popup.svelte';
    import {userData} from '../components/stores'
    const { open } = getContext('simple-modal');
      
      let opening = false;
      let opened = false;
      let closing = false;
      let closed = false;
  
    const showPopup = () => {
        console.log(userData.userName)
        if(userData.userName==undefined){
            //open(Popup, { message: "Login or sign up!" });
        }    
    };
      
  </script>
  
  <section>
      <button on:click={showPopup} type="submit" 
      class="px-6 py-2 mt-4 text-white bg-blue-600 rounded-lg hover:bg-blue-900">Submit!</button>
      <br/>
  
  
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
  </section>

  <style>
	section {
		padding-top: 0.5em;
	}
	
	#state {
		position: absolute;
		top: 0;
		right: 0;
		opacity: 0.33;
		font-size: 0.8em;
	}
</style>