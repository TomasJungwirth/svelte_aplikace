<script>
    let searchQuery = '';
    let items = [
      "Auto",
      "Zvíře",
      "Dům",
      "Rybník",
      "Slunce",
      "Ovoce",
          "Papír",
          "Počítač",
          "Škola",
          "Počasí",
          "Autobus",
          "Prázdniny",
          "Voda",
          "Stůl",
          "Sešit",
          "Brýle",
          "Židle",
          "Selnice",
          "Ovoce",
      "Kolo"
    ];
  
    $: filteredItems = items.filter(item => item.toLowerCase().includes(searchQuery.toLowerCase()));
  </script>
  
  <main>
    <h1>Vyhledávání slov</h1>
    <input type="text" bind:value={searchQuery} placeholder="Hledat slovo..." />
    <ul>
      {#each filteredItems as item}
        <li>{item}</li>
      {/each}
    </ul>
  </main>
  
  <style>
    main {
      text-align: center;
      font-family: Arial, sans-serif;
      padding: 20px;
    }
    input {
      padding: 10px;
      width: 200px;
      margin-bottom: 20px;
      font-size: 16px;
    }
    ul {
      list-style-type: none;
      padding: 0;
    }
    li {
      padding: 5px;
      font-size: 18px;
    }
  </style>
  