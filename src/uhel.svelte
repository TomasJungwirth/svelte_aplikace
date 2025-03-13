<script>
    let angle = 45; // Výchozí úhel
  
    function getCoordinates(angle) {
      let rad = (angle * Math.PI) / 180;
      let x = 100 + 80 * Math.cos(rad);
      let y = 100 - 80 * Math.sin(rad);
      return { x, y };
    }
  
    function getArcPath(angle) {
      let largeArc = angle > 180 ? 1 : 0;
      let coords = getCoordinates(angle);
      return `M 100,100 L 180,100 A 80,80 0 ${largeArc},0 ${coords.x},${coords.y} Z`;
    }
  
    $: coords = getCoordinates(angle);
    $: arcPath = getArcPath(angle);
  </script>
  
  <main>
    <h1>Velikosti úhlu</h1>
  
    <label>Zadej velikost úhlu (°): 
      <input type="number" bind:value={angle} min="0" max="360" step="1" />
    </label>
  
    <svg width="220" height="220">
      
      <circle cx="100" cy="100" r="80" stroke="black" stroke-width="2" fill="none" />
  
      <path d={arcPath} fill="rgba(255, 0, 0, 0.3)" />
  
      <line x1="100" y1="100" x2="180" y2="100" stroke="red" stroke-width="2" />
  
      <line x1="100" y1="100" x2={coords.x} y2={coords.y} stroke="red" stroke-width="2" />
  
      <circle cx="100" cy="100" r="3" fill="black" />
    </svg>
  
    <p><strong>Úhel: {angle}°</strong></p>
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
    }
    svg {
      margin-top: 20px;
      border: 1px solid #ccc;
    }
  </style>
  