<script>
  import { tweened } from 'svelte/motion';

  let partySet = false;
  let roundStarted = false;

  let bank = 0;
  let total = 0;
  let players = 0;

  let paliers = [50, 100, 200, 400, 600, 1000, 3000, 5000];

  $: time = players * 25; // TYPE NUMBER OF SECONDS HERE
	
  let timer;

  setInterval(() => {
    if ($timer > 0 && roundStarted) $timer--;
    if($timer === 0) {
      console.log("piou");
      roundStarted = false;
    }
  }, 1000);
  
  $: minutes = Math.floor($timer / 60);
  $: minname = minutes > 1 ? "mins" : "min";
  $: seconds = Math.floor($timer - minutes * 60)

  function handleSubmit() {
    partySet = true;
    timer = tweened(time)
  }

  function startRound() {
    roundStarted = true;
  }

</script>

<main>
  <h1>Le maillon faible! (version MA viteuf)</h1>
  {#if partySet }
  <div class="container">
    <ul class="paliers">
      {#each paliers as palier, index}
        <li class:palier class:active={index===0}>{palier}</li>
      {/each}
    </ul>
    <div class="main">
      {#if !roundStarted}
        <button on:click={startRound}>Let's play.. the weakest link</button>
      {/if}
      <div>
        Time remaining: <span class="mins">{minutes}</span>{minname} <span class="secs">{seconds}</span>s!
      </div>
      <span>
        round bank: {bank}
        Total Banked : {total}
      </span>
    </div>
  </div>
  {:else}
    <label for="players"> Number of Players</label>
    <input id="players" type="number" bind:value={players} />
    <button disabled={players === 0} on:click={handleSubmit}>Validate</button>
  {/if}
  <p>
    Visit <a href="https://svelte.dev">svelte.dev</a> to learn how to build Svelte
    apps.
  </p>

  <p>
    Check out <a href="https://github.com/sveltejs/kit#readme">SvelteKit</a> for
    the officially supported framework, also powered by Vite!
  </p>
</main>

<style>
  :root {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
      Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
      font-size: 16px;
  }

  .container {
    display: flex;
  }

  .main {
    flex-grow: 1;
  }

  .paliers {
    list-style: none;
    display: flex;
    flex-direction: column-reverse;
  }

  .palier {
    padding: 1rem;
    max-width: 100px;
    border: 1px solid #000;
    margin-bottom: 1rem;
    border-radius: 1rem;;
  }

  .palier.active {
    border-width: 3px;
    background-color: aliceblue;
  }

  main {
    text-align: center;
    padding: 1em;
    margin: 0 auto;
  }

  img {
    height: 16rem;
    width: 16rem;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4rem;
    font-weight: 100;
    line-height: 1.1;
    margin: 2rem auto;
    max-width: 14rem;
  }

  p {
    max-width: 14rem;
    margin: 1rem auto;
    line-height: 1.35;
  }

  @media (min-width: 480px) {
    h1 {
      max-width: none;
    }

    p {
      max-width: none;
    }
  }
</style>
