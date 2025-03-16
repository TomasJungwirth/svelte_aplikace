<script>
  let angle = "45"; 

  function getCoordinates(angle) {
    if (angle === "" || isNaN(angle)) return { x: 100, y: 100 }; // Skryje rafičky
    let rad = (angle * Math.PI) / 180;
    let x = 100 + 80 * Math.cos(rad);
    let y = 100 - 80 * Math.sin(rad);
    return { x, y };
  }

  function getArcPath(angle) {
    if (angle === "" || isNaN(angle) || angle === 0) return ""; // Skryje výseč
    if (angle === 360) return ""; // 360° vykreslíme zvlášť
    let largeArc = angle > 180 ? 1 : 0;
    let coords = getCoordinates(angle);
    return `M 100,100 L 180,100 A 80,80 0 ${largeArc},0 ${coords.x},${coords.y} Z`;
  }

  function validateAngle(event) {
    let value = event.target.value.trim();
    if (value === "") {
      angle = ""; // Nezadán žádný úhel
    } else {
      let num = parseInt(value);
      if (num < 0) angle = 0;
      else if (num > 360) angle = 360;
      else angle = num;
    }
  }

  $: coords = getCoordinates(angle);
  $: arcPath = getArcPath(angle);
</script>




<h1>Příklad úhel</h1>
<p>V tomto příkladu lze zadat velikost úhlu od 0° až po 360°, který se následně vizuálně zobrazí.</p>

<main>
  <h2>Velikost úhlu</h2>

  <label>Zadej velikost úhlu (°): 
    <input type="number" bind:value={angle} min="0" max="360" step="1" on:input={validateAngle} />
  </label>

  <svg width="220" height="220"> // vykreslí plátno
    <circle cx="100" cy="100" r="80" stroke="black" stroke-width="2" fill="none" />  // vykreslí kruh

    {#if angle === 360}
      <circle cx="100" cy="100" r="80" fill="rgba(255, 0, 0, 0.3)" />
    {:else if angle > 0}
      <path d={arcPath} fill="rgba(255, 0, 0, 0.3)" />
    {/if}

    {#if angle !== ""}
      <line x1="100" y1="100" x2="180" y2="100" stroke="red" stroke-width="2" />
      <line x1="100" y1="100" x2={coords.x} y2={coords.y} stroke="red" stroke-width="2" />
    {/if}

    <circle cx="100" cy="100" r="3" fill="black" />
  </svg>

  <p><strong>{angle === "" ? "Úhel nezadán" : `Úhel: ${angle}°`}</strong></p>
</main>



<style>
  main {
    text-align: center;
    font-family: Arial, sans-serif;
  }
  input {
    margin: 10px;
    padding: 5px;
    width: 60px;
    text-align: center;
  }
  svg {
    margin-top: 20px;
    border: 1px solid #ccc;
  }
</style>
