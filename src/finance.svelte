<script>
    import { writable } from 'svelte/store';
  
    let transactions = writable([]);
    let balance = writable(0);
  
    let description = '';
    let amount = 0;
    let type = 'income';
  
    function addTransaction() {
      if (description && amount) {
        transactions.update((currentTransactions) => [
          ...currentTransactions,
          { description, amount: parseFloat(amount), type, id: Date.now() },
        ]);
        updateBalance();
        description = '';
        amount = 0;
      }
    }
  
    function updateBalance() {
      transactions.subscribe((currentTransactions) => {
        let total = 0;
        currentTransactions.forEach((transaction) => {
          if (transaction.type === 'income') {
            total += transaction.amount;
          } else {
            total -= transaction.amount;
          }
        });
        balance.set(total);
      });
    }
  
    function removeTransaction(id) {
      transactions.update((currentTransactions) =>
        currentTransactions.filter((transaction) => transaction.id !== id)
      );
      updateBalance();
    }
  </script>
  
  <style>
    body {
      background-color: #f5f5f5;
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
    }
    
    .container {
      width: 90%;
      max-width: 500px;
      margin: 50px auto;
      padding: 15px;
      background: white;
      border: 1px solid #ddd;
      border-radius: 5px;
      box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.1);
    }
    
    h1 {
      text-align: center;
      color: #333;
    }
    
    .form {
      display: flex;
      flex-direction: column;
      gap: 8px;
    }
    
    input, select, button {
      padding: 8px;
      border: 1px solid #ccc;
      border-radius: 4px;
      font-size: 14px;
    }
    
    button {
      background-color: #4CAF50;
      color: white;
      font-weight: bold;
      cursor: pointer;
      border: none;
      transition: 0.2s;
    }
    
    button:hover {
      background-color: #45a049;
    }
    
    .transactions {
      margin-top: 15px;
    }
    
    .transaction {
      padding: 10px;
      border: 1px solid #ddd;
      border-radius: 4px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 8px;
    }
    
    .income {
      background-color: #dff0d8;
    }
    
    .expense {
      background-color: #f8d7da;
    }
  
    .remove-btn {
      background-color: #d9534f;
      border: none;
      color: white;
      padding: 5px;
      border-radius: 3px;
      cursor: pointer;
    }
  
    .remove-btn:hover {
      background-color: #c9302c;
    }
  </style>
  
  <div class="container">
    <h1>Osobní finance</h1>
  
    <div class="form">
      <input type="text" bind:value={description} placeholder="Popis transakce" />
      <input type="number" bind:value={amount} placeholder="Částka" min="0" />
      <select bind:value={type}>
        <option value="income">Příjem</option>
        <option value="expense">Výdaj</option>
      </select>
      <button on:click={addTransaction}>Přidat transakci</button>
    </div>
  
    <h3 style="text-align:center;">Zůstatek: { $balance } Kč</h3>
  
    <div class="transactions">
      {#each $transactions as { description, amount, type, id }}
        <div class="transaction {type}" key={id}>
          <div>
            <strong>{description}</strong>: {amount} Kč
          </div>
          <button class="remove-btn" on:click={() => removeTransaction(id)}>X</button>
        </div>
      {/each}
    </div>
  </div>
  