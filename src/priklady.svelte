<script>
  let operations = ['+', '-', '*', '/'];
  let currentOperation = operations[0];
  let num1 = Math.floor(Math.random() * 10) + 1;
  let num2 = Math.floor(Math.random() * 10) + 1;
  let userAnswer = '';
  let feedback = '';
  let dobre = 0;
  let spatne = 0;

  function generateProblem() {
    if (currentOperation === '/') {
      num2 = Math.floor(Math.random() * 10) + 1; // Zabráníme dělení nulou
      while (num1 % num2 !== 0) {
        num1 = Math.floor(Math.random() * 100) + 1; // Najdeme dělitelné číslo
      }
    } else {
      num1 = Math.floor(Math.random() * 10) + 1;
      num2 = Math.floor(Math.random() * 10) + 1;
    }
  }

  function spravnaOdpoved() {
    dobre++;
  }

  function spatnaOdpoved() {
    spatne++;
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
    userAnswer = ''; // Vymazání inputu po odpovědi
  }

  function setOperation(operation) {
    currentOperation = operation;
    generateProblem();
  }

  // Dynamická reaktivita: výpočet úspěšnosti
  $: successRate = (dobre + spatne) > 0 ? ((dobre / (dobre + spatne)) * 100).toFixed(2) + "%" : "N/A";

  generateProblem();
</script>

<h1>Příklad příklady</h1>
<p>V tomto příkladu lze zvolit příklady k procvičení - sčítání, odčítání, násobení nebo dělení, podle této volby se budou zobrazovat náhodně vygenerované příklady, po zadání a odeslání výsledku se zobrazí správnost zadaného výsledku. Zároveň je vidět počet spravných a špatných odpovědí, ten se aktualizuje podle zadaných výsledků uživatele.</p>


<h1>Procvičování matematiky</h1>
<p>Vyber co chceš procvičit:</p>

<!-- Tlačítka pro výběr operace -->
<div class="operations">
  <button class="operation-button" on:click={() => setOperation('+')}>Sčítání</button>
  <button class="operation-button" on:click={() => setOperation('-')}>Odčítání</button>
  <button class="operation-button" on:click={() => setOperation('*')}>Násobení</button>
  <button class="operation-button" on:click={() => setOperation('/')}>Dělení</button>
</div><br>

<!-- Zobrazení příkladu -->
<h3 class="problem">{num1} {currentOperation} {num2} = ?</h3>

<!-- Input pro zadání odpovědi -->
<input type="number" bind:value={userAnswer} placeholder="zapiš výsledek" class="answer-input" />
<button class="odpoved" on:click={checkAnswer}>Odpovědět</button>

<!-- Zpětná vazba a statistiky -->
{#if feedback}
  <p><strong>{feedback}</strong></p><br>

  <h3 class="success-rate">Vaše úspěšnost: {successRate}</h3>
  <p>Správně: {dobre}</p>
  <p>Špatně: {spatne}</p>
{/if}

<style>
  main {
    text-align: center;
    font-family: 'Arial', sans-serif;
    padding: 20px;
    background-color: #f7f7f7;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    max-width: 500px;
    margin: 0 auto;
  }

  h1 {
    color: #333;
    font-size: 2rem;
    margin-bottom: 20px;
  }

  p {
    font-size: 1rem;
    color: #555;
  }

  .operations {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
    gap: 10px; /* Zkrátí mezery mezi tlačítky */
  }

  .operation-button {
    padding: 12px 20px;
    font-size: 16px;
    background-color: #4A90E2; /* Modrá barva */
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    width: 22%; /* Omezí šířku tlačítek */
  }

  .operation-button:hover {
    background-color: #357ABD; /* Tmavší modrá při najetí myší */
  }

  .answer-input {
    padding: 10px;
    font-size: 16px;
    margin-top: 20px;
    width: 25%;
    border-radius: 5px;
    border: 1px solid #ccc;
  }

  .odpoved {
    padding: 10px 20px;
    background-color: #66bb6a;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    margin-top: 20px;
    font-size: 16px;
  }

  .odpoved:hover {
    background-color: #55a44d;
  }

  .success-rate {
    font-size: 1.2rem;
    font-weight: bold;
    margin-top: 10px;
  }

  h3 {
    color: #333;
  }
</style>
