<script>
    let initialAnimals = [
        { name: "Kočka", img: "https://upload.wikimedia.org/wikipedia/commons/thumb/b/b6/Felis_catus-cat_on_snow.jpg/200px-Felis_catus-cat_on_snow.jpg", id: 1 },
        { name: "Pes", img: "https://upload.wikimedia.org/wikipedia/commons/3/32/Bloodhound_423.jpg", id: 2 },
        { name: "Slon", img: "https://upload.wikimedia.org/wikipedia/commons/thumb/3/37/African_Bush_Elephant.jpg/200px-African_Bush_Elephant.jpg", id: 3 },
        { name: "Kůň", img: "https://upload.wikimedia.org/wikipedia/commons/1/1c/Jerome_Kinsky_Zlosyn_2005.jpg", id: 4 },
        { name: "Žába", img: "https://upload.wikimedia.org/wikipedia/commons/c/c2/Litoria_caerulea.jpg", id: 5 },
        { name: "Lev", img: "https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Lion_waiting_in_Namibia.jpg/200px-Lion_waiting_in_Namibia.jpg", id: 6 },
        { name: "Kráva", img: "https://upload.wikimedia.org/wikipedia/commons/4/4c/Tur_domaci.jpg", id: 7 },
        { name: "Kachna", img: "https://upload.wikimedia.org/wikipedia/commons/4/47/Kachna.jpg", id: 8 }
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
            if (draggedItem.id === targetAnimal.id) {
                results[targetAnimal.id] = true;
                remainingNames = remainingNames.filter(a => a.id !== draggedItem.id);
            } else {
                results[targetAnimal.id] = false;
            }
            draggedItem = null;
        }
    }

    function resetGame() {
        animals = [...initialAnimals];
        remainingNames = [...initialAnimals];
        results = {};
    }
</script>

<style>
    .container { display: flex; flex-wrap: wrap; justify-content: center; gap: 15px; margin-top: 20px; }
    .box { width: 140px; height: 140px; border: 2px dashed gray; display: flex; align-items: center; justify-content: center; }
    .img { width: 100%; height: 100%; object-fit: cover; border-radius: 10px; }
    .name { padding: 10px; background: lightgray; cursor: grab; width: 120px; text-align: center; border-radius: 5px; }
    .correct { background: lightgreen; border: 3px solid green;}
    .wrong { background: lightcoral; border: 3px solid red;}
    button { margin-top: 20px; padding: 10px 15px; font-size: 16px; cursor: pointer; border: none; background: dodgerblue; color: white; border-radius: 5px; }
    button:hover { background: darkblue; }
</style>

<h3>Přetáhni správné jméno ke zvířeti</h3>

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
