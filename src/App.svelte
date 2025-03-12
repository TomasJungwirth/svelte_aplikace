<script>
	import Vlajky from "./vlajky.svelte";
	  import Kviz from "./kviz.svelte";
  
	let selectedComponent = null;
	let darkMode = false;
  
	function handleClick(component) {
	  selectedComponent = component;
	}
  
	function goBack() {
	  selectedComponent = null;
	}
  </script>
  
  <button on:click={() => darkMode = !darkMode}>
	{darkMode ? "Světlý" : "Tmavý"} režim
  </button>
  
  {#if darkMode}
	<script>document.body.classList.add("dark")</script>
  {:else}
	<script>document.body.classList.remove("dark")</script>
  {/if}
  
  <main class="container">
	<!-- Hlavní nadpis se mění podle obsahu -->
	<h1>
	  {#if selectedComponent === 'Vlajky'}
		Vlajky
	  {:else if selectedComponent}
		{selectedComponent}
	  {:else}
		Interaktivní výuková aplikace
	  {/if}
	</h1>
  
	<!-- Zobrazení tlačítka zpět, pokud je vybraná komponenta -->
	{#if selectedComponent}
	  <button class="back-button" on:click={goBack}>Zpátky</button>
	{/if}
  
	{#if !selectedComponent} 
	  <!-- Menu tlačítek -->
	  <div class="buttons">
		<button on:click={() => handleClick('Vlajky')}>Vlajky</button>
		<button on:click={() => handleClick('Kvíz')}>Kvíz</button>
		<button on:click={() => handleClick('Matematika')}>Matematika</button>
		<button on:click={() => handleClick('Čeština')}>Čeština</button>
		<button on:click={() => handleClick('Fyzika')}>Fyzika</button>
	  </div>
	{/if}
  
	<!-- Dynamické načítání komponenty -->
	{#if selectedComponent === 'Vlajky'}
	  <Vlajky />
	{/if}
  
	  {#if selectedComponent === 'Kvíz'}
	  <Kviz />
	{/if}
  
	  
  </main>
  
  <style>
	.container {
	  text-align: center;
	  font-family: Arial, sans-serif;
	  max-width: 600px;
	  margin: 0 auto;
	  padding: 20px;
	}
  
	h1 {
	  color: #2c3e50;
	}
  
	.buttons {
	  display: flex;
	  flex-direction: column;
	  gap: 10px;
	}
  
	button {
	  background-color: #3498db;
	  color: white;
	  border: none;
	  padding: 10px 15px;
	  font-size: 1em;
	  cursor: pointer;
	  border-radius: 5px;
	  transition: background 0.3s;
	}
  
	button:hover {
	  background-color: #2980b9;
	}
  
	/* Styl pro tlačítko zpět */
	.back-button {
	  margin-bottom: 15px;
	  background-color: #e74c3c;
	}
  
	.back-button:hover {
	  background-color: #c0392b;
	}
  
	body { background: white; color: black; }
	:global(body.dark) { background: black; color: white; }
  </style>
  