<script>
    // Pole pro uložení barev každého čtverce
    let squares = Array(9).fill('white'); // počáteční barvy všech čtverců jsou bílé
  
    // Vybraná barva z palety
    let selectedColor = 'blue'; // výchozí barva je černá
  
    // Pole pro sledování použitých barev
    let usedColors = [];
  
    // Seznam barev pro paletu
    let colorPalette = ['blue', 'yellow', 'green', 'orange', 'brown', 'red', 'grey', 'black', 'pink'];
  
    // Funkce pro změnu barvy čtverce
    function changeColor(index) {
      // Změní barvu čtverce a přidá ji do seznamu použitých barev
      squares[index] = selectedColor;
      usedColors.push(selectedColor);
      // Odstraní barvu z palety
      colorPalette = colorPalette.filter(color => color !== selectedColor);
    }
  
    // Funkce pro výběr barvy z palety
    function selectColor(color) {
      // Nastaví vybranou barvu, pokud není již použita
      if (!usedColors.includes(color)) {
        selectedColor = color;
      }
    }
  
    // Funkce pro resetování kostky
    function reset() {
      // Obnoví všechny čtverce na původní bílé barvy
      squares = Array(9).fill('white');
      // Obnoví seznam použitých barev
      usedColors = [];
      // Obnoví paletu barev
      colorPalette = ['blue', 'yellow', 'green', 'orange', 'brown', 'red', 'grey', 'black', 'pink'];
      // Resetuje vybranou barvu na výchozí hodnotu (například černou)
      selectedColor = 'black';
    }
  </script>
  
  <style>
    .container {
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      padding: 20px;
    }
  
    .cube {
      display: grid;
      grid-template-columns: repeat(3, 1fr); /* 3 sloupce pro kostku */
      grid-template-rows: repeat(3, 1fr); /* 3 řádky pro kostku */
      width: 300px; /* šířka kostky */
      height: 300px; /* výška kostky */
      gap: 2px; /* mezera mezi čtverci */
      margin-right: 20px;
    }
  
    .square {
      display: flex;
      justify-content: center;
      align-items: center;
      border: 1px solid #ccc;
      width: 100%;
      height: 100%;
      cursor: pointer;
    }
  
    .palette {
      display: grid;
      grid-template-columns: repeat(3, 1fr); /* 3 sloupce pro paletu barev */
      gap: 10px;
      width: 150px;
    }
  
    .palette div {
      width: 40px;
      height: 40px;
      border-radius: 50%; /* Tvoří kruh */
      cursor: pointer;
      border: 2px solid #ccc;
      margin: 5px;
    }
  
    .palette div.disabled {
      opacity: 0.3; /* Neaktivní barvy budou méně viditelné */
      pointer-events: none; /* Zamezí kliknutí na neaktivní barvy */
    }
  
    .reset-button {
      margin-top: 20px;
      padding: 10px;
      background-color: black; /* Červené tlačítko */
      color: white;
      border: none;
      cursor: pointer;
      font-size: 16px;
    }
  
    .reset-button:hover {
      background-color: #d32f2f;
    }
  
    .instruction-text {
      font-size: 18px;
      font-weight: bold;
      margin-bottom: 10px;
      color: #333;
    }
  
    .reset-container {
      display: flex;
      justify-content: center;
      margin-top: 20px;
    }
  </style>
  
  <div class="container">
    <!-- kostka -->
    <div>
      <div class="instruction-text">Vybarvi kostku podle zadání</div>
      <div class="cube">
        {#each squares as square, index}
          <div class="square" style="background-color: {square}" on:click={() => changeColor(index)}>
          </div>
        {/each}
      </div>
    </div>
  
  
    <div>
      <div class="instruction-text">Zvolte barvu</div>
      <div class="palette">
        {#each colorPalette as color}
          <div 
            style="background-color: {color};" 
            on:click={() => selectColor(color)}
            class={usedColors.includes(color) ? 'disabled' : ''}
            title={color}>
          </div>
        {/each}
      </div>
    </div>
  </div>
  
  
  <div class="reset-container">
    <button class="reset-button" on:click={reset}>Reset</button>
  </div>
  