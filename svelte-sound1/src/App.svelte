<script>
  // Array of vowel labels for the first 9 buttons
  const vowels = ['a', 'e', 'i', 'o', 'u', 'y', 'å', 'ä', 'ö'];

  // All buttons: 9 vowels + 1 "All" button
  const buttons = [...vowels, 'All'];

  // Track currently playing audio and whether to stop the sequence
  let currentAudio = null;
  let stopSequence = false;

  // Function to stop any currently playing audio
  function stopCurrentAudio() {
    if (currentAudio) {
      currentAudio.pause();
      currentAudio.currentTime = 0;
      currentAudio = null;
    }
    stopSequence = true;
  }

  // Function to play the corresponding sound
  async function playSound(label) {
    // Stop any currently playing audio first
    stopCurrentAudio();

    if (label === 'All') {
      stopSequence = false;
      // Play all sounds in sequence with 0.5 second pause between each
      for (const vowel of vowels) {
        if (stopSequence) break; // Stop if another button was clicked

        currentAudio = new Audio(`/vokaler_ludvig/${vowel}.opus`);
        // Wait for the audio to finish playing
        await new Promise(resolve => {
          currentAudio.onended = resolve;
          currentAudio.play();
        });

        if (stopSequence) break; // Stop if another button was clicked during playback

        // Wait for 0.5 seconds before playing the next sound
        await new Promise(resolve => setTimeout(resolve, 500));
      }
      currentAudio = null;
    } else {
      currentAudio = new Audio(`/vokaler_ludvig/${label}.opus`);
      currentAudio.play();
    }
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
