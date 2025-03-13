<script>
    let operations = ['+', '-', '*', '/'];
    let currentOperation = operations[0];
    let num1 = Math.floor(Math.random() * 10) + 1;
    let num2 = Math.floor(Math.random() * 10) + 1;
    let answer = '';
    let userAnswer = '';
    let feedback = '';
      let dobre = 0;
      let spatne = 0;
  
    function generateProblem() {
      if (currentOperation === '/') {
        num2 = Math.floor(Math.random() * 10) + 1; // Zajistíme, že dělení nebude nulové
        while (num1 % num2 !== 0) {
          num1 = Math.floor(Math.random() * 100) + 1; // Vygenerujeme tak, aby bylo dělení celé
        }
      } else {
        num1 = Math.floor(Math.random() * 10) + 1;
        num2 = Math.floor(Math.random() * 10) + 1;
      }
    }
      function spravnaOdpoved() {
          dobre = dobre + 1;
      }
  
      function spatnaOdpoved() {
          spatne = spatne + 1;
      }
  
    function checkAnswer() {
      let correctAnswer;
  
      if (currentOperation === '+') correctAnswer = num1 + num2;
      if (currentOperation === '-') correctAnswer = num1 - num2;
      if (currentOperation === '*') correctAnswer = num1 * num2;
      if (currentOperation === '/') correctAnswer = num1 / num2;
  
      if (parseFloat(userAnswer) === correctAnswer) {
        feedback = 'To je správně! Jen tak dál!';
              spravnaOdpoved();
      } else {
        feedback = `Špatně, správná odpověď je ${correctAnswer}.`;
              spatnaOdpoved();
      }
      generateProblem();
    }
  
    function setOperation(operation) {
      currentOperation = operation;
      generateProblem();
    }
  
    // Inicializace problému při načtení komponenty
    generateProblem();
  </script>
  
  <main>
    <h1>Procvičování matematiky</h1>
    <p>Vyber příklady:</p>
    <button on:click={() => setOperation('+')}>Sčítání</button>
    <button on:click={() => setOperation('-')}>Odčítání</button>
    <button on:click={() => setOperation('*')}>Násobení</button>
    <button on:click={() => setOperation('/')}>Dělení</button>
  
    <p>{num1} {currentOperation} {num2} = ?</p>
    <input type="number" bind:value={userAnswer} placeholder="zapiš výsledek" />
    <button class="odpoved" on:click={checkAnswer}>Odpovědět</button>
      
  
    {#if feedback}
      <p><strong>{feedback}</strong></p><br>
          <p>Správně: {dobre}</p>
          <p>Špatně: {spatne}</p>
    {/if}
  </main>
  
  <style>
    main {
      text-align: center;
      font-family: Arial, sans-serif;
      padding: 20px;
    }
    input, button {
      padding: 10px;
      margin: 10px;
      font-size: 16px;
    }
    button {
      background-color: skyblue;
      cursor: pointer;
    }
    button:hover {
      background-color: slategray;
    }
  
      .odpoved {
      background-color: lawngreen;
      cursor: pointer;
    }
  
      .odpoved:hover {
      background-color: limegreen;
    }
  </style>
  