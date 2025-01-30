<script>
    import { onMount } from 'svelte';
    let responseMessage = '';
    let showInputPanel = false;
    let userMessage = '';
	let msg2 = ':json;dc=@datacache;model = \'C:/models/plsmodel.mat\';data=dc.data;pred = data | model; pred=pred.pred{2};pred';
	let msg1 = ':json;:whos;';

    function openInputPanel() {
        showInputPanel = true;
    }

    async function sendRequest(msg) {
        try {
		    console.log('sendRequest: msg = ' + msg);
            const msgToSend = encodeURIComponent(msg.trim());
            const res = await fetch(`http://127.0.0.1:2211/?${msgToSend}`);
            const data = await res.json(); // Assuming API returns JSON
			//const data = await res.text();
            //responseMessage = data;

		    console.log('sendRequest, data = ' + data);
		    console.log('sendRequest, data.response.error = ' + data.response.error);
		    console.log('sendRequest, data.response.result = ' + data.response.result);
			let ttype = typeof  data.response.result;
			console.log('typeof data.response.result = ' + ttype);
            // Check for errors in the response
            if (data.response.error) {
                responseMessage = `Error: ${data.response.error}`;
            } else {
                responseMessage = data.response.result || 'No result available';
            }

            showInputPanel = false; // Hide the input panel after request
        } catch (error) {
            responseMessage = 'Error: ' + error.message;
        }
    }

    // For testing: automatically send the "msg1" request
    onMount(() => {
        sendRequest(msg1);
    });
</script>

<style>
    .container {
        display: flex;
        background-color: lightblue;
        padding: 20px;
        width: 100%;
        height: 100%;
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
        <button on:click={() => sendRequest(msg2)}>Send Msg2</button>
    </div>
    
    {#if showInputPanel}
        <div class="input-panel">
            <input type="text" bind:value={userMessage} placeholder="Enter your message here" />
            <button on:click={() => sendRequest(userMessage)}>Submit</button>
        </div>
    {/if}
    
    <div class="response-container">
        <textarea rows="10" cols="30" bind:value={responseMessage} readonly></textarea>
    </div>
</div>
