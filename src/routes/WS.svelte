<script>
    import { onMount } from 'svelte';
  
    let ipAddress = '';
    let port = '4444';
    let webSocket;
    let savedIPs = [];
 
    let error = false;
    
    function connect() {
      if (ipAddress.trim() === '') {
        alert('Please enter an IP address.');
        return;
      }
  
      webSocket = new WebSocket(`ws:\\${ipAddress}:${port}`);
      // Handle WebSocket events and logic here

      webSocket.onerror = (e) => {
        console.log(e);
        error = true;
      }
    }
  
    function disconnect() {
      if (webSocket) {
        webSocket.close();
        webSocket = null;
      }
    }
  
    function saveToLocalStorage() {
      if (ipAddress.trim() === '') {
        alert('Please enter an IP address.');
        return;
      }
  
      localStorage.setItem('ipAddress', ipAddress);
      loadSavedIPs();
    //   alert('IP address saved to localStorage!');
    }
  
    function loadSavedIPs() {
      savedIPs = Object.values(localStorage);
    }
  
    function removeFromLocalStorage(ip) {
      if (savedIPs.includes(ip)) {
        localStorage.removeItem('ipAddress');
        loadSavedIPs();
        // alert('IP address removed from localStorage!');
      }
    }
  
    onMount(() => {
      loadSavedIPs();
      if (savedIPs.length > 0) {
        ipAddress = savedIPs[0];
      }
    });
  </script>
  
  <div class={error ?  'error' : ''}>
  
    <label for="ipAddress">IP Address:</label>
    <input type="text" id="ipAddress" bind:value={ipAddress} placeholder="Enter IP address" />

    <label for="port">Port:</label>
    <input type="text" id="port" bind:value={port} placeholder="Enter Port" />

    <button on:click={connect}>Connect</button>
    <button on:click={disconnect}>Disconnect</button>
    <button on:click={saveToLocalStorage}>Save to LocalStorage</button>
  
    {#if savedIPs.length > 0}
      <select bind:value={ipAddress}>
        {#each savedIPs as ip}
          <option value={ip}>{ip}</option>
        {/each}
      </select>
      <button on:click={() => removeFromLocalStorage(ipAddress)}>Remove from LocalStorage</button>
    {/if}
    </div>
  
    <style>
        .error{
            background-color: #F00;
        }
    </style>