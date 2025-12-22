<script>
  // Array of button labels
  const buttons = Array.from({ length: 10 }, (_, i) => `A${i + 1}`);

  // Function to play the corresponding sound
  function playSound(label) {
    const audio = new Audio(`/mySounds/${label}.mp3`);
    audio.play();
  }
</script>

<style>
  body {
    margin: 0;
  }

  main {
    background-color: lightblue;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 1rem;
  }

  .buttons {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    justify-content: center;
  }

  button {
    background-color: yellow;
    border: none;
    padding: 1rem 2rem;
    font-size: 1.2rem;
    cursor: pointer;
    border-radius: 5px;
    transition: transform 0.1s;
  }

  button:active {
    transform: scale(0.95);
  }
</style>

<main>
  <h1>Svelte Sound SPA</h1>
  <div class="buttons">
    {#each buttons as button}
      <button on:click={() => playSound(button)}>
        {button}
      </button>
    {/each}
  </div>
</main>
