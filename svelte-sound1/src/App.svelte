<script>
  // Password protection
  const CORRECT_PASSWORD = 'Kalmar1';
  let isAuthenticated = false;
  let passwordInput = '';
  let passwordError = false;

  // Check if user has already authenticated
  if (typeof window !== 'undefined') {
    isAuthenticated = localStorage.getItem('vokaler_auth') === 'true';
  }

  function checkPassword() {
    if (passwordInput === CORRECT_PASSWORD) {
      isAuthenticated = true;
      localStorage.setItem('vokaler_auth', 'true');
      passwordError = false;
    } else {
      passwordError = true;
    }
  }

  function handleKeyPress(event) {
    if (event.key === 'Enter') {
      checkPassword();
    }
  }

  // Derive the speaker name from the sound folder path
  const soundFolder = 'vokaler_ludvig';
  const speakerName = soundFolder.split('_')[1].charAt(0).toUpperCase() + soundFolder.split('_')[1].slice(1);
  const pageTitle = `Svenska Vokaler (${speakerName})`;

  // Array of vowel labels for the first 9 buttons
  const vowels = ['a', 'e', 'i', 'o', 'u', 'y', 'å', 'ä', 'ö'];

  // Text labels for each button
  const longLabels = ['apa', 'elbil', 'ishockey', 'odla', 'utan', 'ytlig', 'åsikt', 'äventyr', 'öde', ' '];
  const shortLabels = ['alla', 'eld', 'ilska', 'oktober', 'ursäkta', 'yrke', 'ånga', 'ängel', 'önska', ' '];

  // Create button data with labels
  const allButtons = [...vowels, 'All'].map((button, index) => ({
    label: button,
    longText: longLabels[index],
    shortText: shortLabels[index]
  }));

  // Split into two rows of 5 buttons each
  const row1 = allButtons.slice(0, 5);  // a, e, i, o, u
  const row2 = allButtons.slice(5);     // y, å, ä, ö, All

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

        currentAudio = new Audio(`${soundFolder}/${vowel}.opus`);
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
      currentAudio = new Audio(`${soundFolder}/${label}.opus`);
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

  .button-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.3rem;
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

  .button-text {
    display: flex;
    flex-direction: column;
    align-items: center;
    font-size: calc(min(2.4rem, 4vw) * 0.7);
    color: inherit;
    line-height: 1.2;
  }

  .button-text-line {
    min-height: 1.2em;
  }

  .password-screen {
    background-color: #FECC00;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 1.5rem;
  }

  .password-container {
    background-color: white;
    padding: 3rem;
    border-radius: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    text-align: center;
  }

  .password-container h2 {
    margin-top: 0;
    color: #333;
  }

  .password-container input {
    padding: 0.8rem;
    font-size: 1.2rem;
    border: 2px solid #ccc;
    border-radius: 5px;
    width: 250px;
    margin-bottom: 1rem;
  }

  .password-container input:focus {
    outline: none;
    border-color: lightblue;
  }

  .password-container button {
    background-color: lightblue;
    color: #333;
    padding: 0.8rem 2rem;
    font-size: 1.2rem;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  .password-container button:hover {
    background-color: #add8e6;
  }

  .error-message {
    color: red;
    margin-top: 0.5rem;
    font-weight: bold;
  }
</style>

{#if !isAuthenticated}
  <div class="password-screen">
    <div class="password-container">
      <h2>{pageTitle}</h2>
      <p>Please enter the password to access this application:</p>
      <input
        type="password"
        bind:value={passwordInput}
        on:keypress={handleKeyPress}
        placeholder="Enter password"
        autofocus
      />
      <br>
      <button on:click={checkPassword}>Submit</button>
      {#if passwordError}
        <p class="error-message">Incorrect password. Please try again.</p>
      {/if}
    </div>
  </div>
{:else}
  <main>
    <h1>{pageTitle}</h1>
    <div class="buttons-container">
      <div class="button-row">
        {#each row1 as buttonData}
          <div class="button-wrapper">
            <button on:click={() => playSound(buttonData.label)}>
              {buttonData.label}
            </button>
            <div class="button-text">
              <div class="button-text-line">{buttonData.longText}</div>
              <div class="button-text-line">{buttonData.shortText}</div>
            </div>
          </div>
        {/each}
      </div>
      <div class="button-row">
        {#each row2 as buttonData}
          <div class="button-wrapper">
            <button on:click={() => playSound(buttonData.label)}>
              {buttonData.label}
            </button>
            <div class="button-text">
              <div class="button-text-line">{buttonData.longText}</div>
              <div class="button-text-line">{buttonData.shortText}</div>
            </div>
          </div>
        {/each}
      </div>
    </div>
  </main>
{/if}