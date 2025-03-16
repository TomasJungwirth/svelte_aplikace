<script>
	import Vlajky from "./vlajky.svelte"; //vytvořit kopii
	  import Kviz from "./kviz.svelte"; 
	  import Priklady from "./priklady.svelte";
	  import Obrazky from "./obrazky.svelte"; //vytvořit kopii
	  import Uhel from "./uhel.svelte";
	  import Finance from "./finance.svelte"; 
  
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
	{#if selectedComponent}
	  <button class="back-button" on:click={goBack}>Zpátky</button>
	{/if}
  
	{#if !selectedComponent} 
		  <img class="logo" src="https://upload.wikimedia.org/wikipedia/commons/1/1b/Svelte_Logo.svg" alt="logo frameworku Svelte"/>
		  <h1>Interaktivní výuková aplikace</h1>
		  <p>Tato aplikace je vytvořena pomocí frameworku Svelte, obsahuje 6 vzorových příkladů, kde každý z nich demonstruje schopnosti frameworku ve vztahu k vytváření interaktivních výukových aplikací</p><br>
	  <div class="buttons">
		<button on:click={() => handleClick('Vlajky')}>Vlajky</button>
		<button on:click={() => handleClick('Kviz')}>Kvíz</button>
		<button on:click={() => handleClick('Priklady')}>Příklady</button>
		<button on:click={() => handleClick('Obrazky')}>Obrázky</button>
			  <button on:click={() => handleClick('Uhel')}>Úhel</button>
			  <button on:click={() => handleClick('Finance')}>Finance</button>
	  </div>
	{/if}
  
	<!-- Podmínka pro volání komponent -->
	{#if selectedComponent === 'Vlajky'}
	  <Vlajky />
	  {:else if selectedComponent === 'Kviz'}
		  <Kviz />
	  {:else if selectedComponent === 'Priklady'}
		  <Priklady />
	  {:else if selectedComponent === 'Obrazky'}
		  <Obrazky />
	  {:else if selectedComponent === 'Uhel'}
		  <Uhel />
	  {:else if selectedComponent === 'Finance'}
		  <Finance />
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
  
	.buttons {
	  display: flex;
	  flex-direction: column;
	  gap: 10px;
	}
  
	  .logo  {
	  width: 13%;
	}
  
	button {
	  background-color: darkorange;
	  color: white;
	  border: none;
	  padding: 10px 15px;
	  font-size: 1em;
	  cursor: pointer;
	  border-radius: 5px;
	  transition: background 0.3s;
	}
  
	button:hover {
	  background-color: orangered;
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
  