<script>
    import { onMount } from 'svelte';
    let responseMessage = '';
    let showInputPanel = false;
    let userMessage = '';
    
    function openInputPanel() {
        showInputPanel = true;
    }
    
    async function sendRequest() {
        try {
            const msg = encodeURIComponent(userMessage.trim());
            const res = await fetch(`http://127.0.0.1:2211/?${msg}`);
            const data = await res.text();
            responseMessage = data;
            showInputPanel = false;
        } catch (error) {
            responseMessage = 'Error: ' + error.message;
        }
    }
</script>

<style>
    .container {
        display: flex;
        background-color: lightblue;
        padding: 20px;
        width: 100%;
        height: 400px;
    }
    .button-container, .input-panel {
        flex: 1;
    }
    .response-container {
        flex: 1;
        margin-left: 20px;
    }
</style>

<div class="container">
    <div class="button-container" style="display: {showInputPanel ? 'none' : 'block'};">
        <button on:click={openInputPanel}>Go</button>
    </div>
    
    {#if showInputPanel}
        <div class="input-panel">
            <input type="text" bind:value={userMessage} placeholder="Enter your message here" />
            <button on:click={sendRequest}>Submit</button>
        </div>
    {/if}
    
    <div class="response-container">
        <textarea rows="10" cols="30" bind:value={responseMessage} readonly></textarea>
    </div>
</div>
