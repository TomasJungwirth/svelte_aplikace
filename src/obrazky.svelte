<script>
    let initialAnimals = [
        { name: "Kočka", img: "https://upload.wikimedia.org/wikipedia/commons/thumb/b/b6/Felis_catus-cat_on_snow.jpg/200px-Felis_catus-cat_on_snow.jpg", id: 1 },
        { name: "Jelen", img: "https://upload.wikimedia.org/wikipedia/commons/d/d9/Largest_Red_Deer.jpg", id: 2 },
        { name: "Slon", img: "https://upload.wikimedia.org/wikipedia/commons/thumb/3/37/African_Bush_Elephant.jpg/200px-African_Bush_Elephant.jpg", id: 3 },
        { name: "Žirafa", img: "https://upload.wikimedia.org/wikipedia/commons/9/9e/Rothschild%27s_Giraffe_%28Giraffa_camelopardalis_rothschildi%29_male_%287068054987%29.jpg", id: 4 },
        { name: "Lev", img: "https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Lion_waiting_in_Namibia.jpg/200px-Lion_waiting_in_Namibia.jpg", id: 5 },
        { name: "Kráva", img: "https://upload.wikimedia.org/wikipedia/commons/4/4c/Tur_domaci.jpg", id: 6 },


		
    ];

    let animals = [...initialAnimals];
    let remainingNames = [...initialAnimals];
    let results = {};
    let draggedItem = null;

    function handleDragStart(event, animal) {
        draggedItem = animal;
    }

   function handleDrop(event, targetAnimal) {
    if (draggedItem) {
        // Pokud je obrázek již správně spojen, nedovolíme další změnu
        if (results[targetAnimal.id] === true) {
            return;
        }

        if (draggedItem.id === targetAnimal.id) {
            results[targetAnimal.id] = true;
            remainingNames = remainingNames.filter(a => a.id !== draggedItem.id);
        } else {
            results[targetAnimal.id] = false;
        }

        draggedItem = null;
    }
}

	function shuffle(array) {
    for (let i = array.length - 1; i > 0; i--) {
        let j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
    }
}

 function resetGame() {
        animals = [...initialAnimals];
        remainingNames = [...initialAnimals];
        results = {};
        shuffle(animals);  // 🔄 Zamícháme obrázky
        shuffle(remainingNames);  // 🔄 Zamícháme názvy
    }

    resetGame(); // 🔄 Zamícháme i při načtení hry
</script>




<style>
    .container { display: flex; flex-wrap: wrap; justify-content: center; gap: 15px; margin-top: 20px; }
    .box { width: 140px; height: 140px; border: 2px dashed gray; display: flex; align-items: center; justify-content: center; }
    .img { width: 100%; height: 100%; object-fit: cover; border-radius: 10px; }
    .name { padding: 10px; background: lightgray; cursor: grab; width: 120px; text-align: center; border-radius: 5px; }
    .correct { background: lightgreen; border: 4px solid green;}
    .wrong { background: lightcoral; border: 4px solid red;}
    button { margin-top: 20px; padding: 10px 15px; font-size: 16px; cursor: pointer; border: none; background: dodgerblue; color: white; border-radius: 5px; }
    button:hover { background: darkblue; }
</style>

<h1>Příklad obrázky</h1>
<p>V této úloze se přetahují názvy zvířat k odpovídajícímu obrázku, pokud se spojí název se správným obrázkem, vytvoří se kolem obrázku zelený rámeček v opačném případě se vytvoří červený, který značí nesprávnou odpověď. Po kliknutí na tlačítko „Opakovat“ se úloha vrátí na začátek, ale zamíchají se obrázky i názvy zvířat. Obrázky zvířat byly převzaty z Wikipedie.</p>


<h3>Přetáhni správný název k obrázku zvířete</h3>

<div class="container">
    {#each animals as animal}
        <div 
            class="box {results[animal.id] === true ? 'correct' : results[animal.id] === false ? 'wrong' : ''}" 
            on:dragover|preventDefault 
            on:drop={event => handleDrop(event, animal)}
        >
            <img src={animal.img} alt={animal.name} class="img" />
        </div>
    {/each}
</div>

<div class="container">
    {#each remainingNames as animal}
        <div class="name" draggable="true" on:dragstart={event => handleDragStart(event, animal)}>
            {animal.name}
        </div>
    {/each}
</div>

<button on:click={resetGame}>Opakovat</button>
