<script>
  // Array of vowel labels for the first 9 buttons
  const vowels = ['a', 'e', 'i', 'o', 'u', 'y', 'å', 'ä', 'ö'];

  // Split into two rows of 5 buttons each
  const row1 = vowels.slice(0, 5);  // a, e, i, o, u
  const row2 = [...vowels.slice(5), 'All'];  // y, å, ä, ö, All

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
    background-color: #FECC00;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 1rem;
  }

  .buttons-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1.5rem;
  }

  .button-row {
    display: flex;
    gap: 1.5vw;
    justify-content: center;
    width: 100%;
  }


  button {
    background-color: lightblue;
    border: none;
    padding: min(2rem, 3vh) min(4rem, 8vw);
    font-size: min(2.4rem, 4vw);
    cursor: pointer;
    border-radius: 5px;
    transition: transform 0.1s;
    flex-shrink: 1;
    white-space: nowrap;
  }

  button:active {
    transform: scale(0.95);
  }
</style>

<main>
  <h1>Vokaler_Ludvig</h1>
  <div class="buttons-container">
    <div class="button-row">
      {#each row1 as button}
        <button on:click={() => playSound(button)}>
          {button}
        </button>
      {/each}
    </div>
    <div class="button-row">
      {#each row2 as button}
        <button on:click={() => playSound(button)}>
          {button}
        </button>
      {/each}
    </div>
  </div>
</main>
