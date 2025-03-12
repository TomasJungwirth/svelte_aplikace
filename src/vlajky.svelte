<script>
    import { onMount } from 'svelte';
  
    let selectedContinent = '';
    let flags = [];
    let selectedCountry = null;
  
    // Seznam států a jejich vlajek
    const continents = {
      "Evropa": [
        { name: "Česká republika", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/c/cb/Flag_of_the_Czech_Republic.svg" },
        { name: "Německo", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/b/ba/Flag_of_Germany.svg" },
        { name: "Itálie", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/0/03/Flag_of_Italy.svg" },
        { name: "Francie", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/c/c3/Flag_of_France.svg" },
        { name: "Norsko", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/d/d9/Flag_of_Norway.svg" }
      ],
      "Afrika": [
        { name: "Egypt", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/f/fe/Flag_of_Egypt.svg" },
        { name: "Nigérie", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/7/79/Flag_of_Nigeria.svg" },
        { name: "Jihoafrická republika", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/a/af/Flag_of_South_Africa.svg" },
        { name: "Keňa", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/4/49/Flag_of_Kenya.svg" },
        { name: "Tanzanie", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/3/38/Flag_of_Tanzania.svg" }
      ],
      "Asie": [
        { name: "Čína", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/f/fa/Flag_of_the_People%27s_Republic_of_China.svg" },
        { name: "Indie", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/4/41/Flag_of_India.svg" },
        { name: "Japonsko", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/9/9e/Flag_of_Japan.svg" },
        { name: "Thajsko", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/a/a9/Flag_of_Thailand.svg" },
        { name: "Vietnam", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/2/21/Flag_of_Vietnam.svg" }
      ],
      "Severní Amerika": [
        { name: "USA", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/a/a4/Flag_of_the_United_States.svg" },
        { name: "Kanada", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/c/cf/Flag_of_Canada.svg" },
        { name: "Mexiko", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/f/fc/Flag_of_Mexico.svg" },
        { name: "Kuba", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/b/bd/Flag_of_Cuba.svg" },
        { name: "Dominikánská republika", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/9/9f/Flag_of_the_Dominican_Republic.svg" }
      ],
      "Jižní Amerika": [
        { name: "Brazílie", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/0/05/Flag_of_Brazil.svg" },
        { name: "Argentina", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/1/1a/Flag_of_Argentina.svg" },
        { name: "Kolumbie", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/2/21/Flag_of_Colombia.svg" },
        { name: "Chile", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/7/78/Flag_of_Chile.svg" },
        { name: "Peru", flagUrl: "https://upload.wikimedia.org/wikipedia/commons/d/df/Flag_of_Peru_%28state%29.svg" }
      ]
    };
  
    // Mapa států a jejich informací
    const countries = {
      "Česká republika": { name: "Česká republika", info: "Praha", population: "10,9 milionu obyvatel", rozloha: "78 870 km²", reky: "Labe, Vltava" },
      "Německo": { name: "Německo", info: "Berlín", population: "84,4 milionů obyvatel", rozloha: "357 588 km²", reky: "Rýn, Dunaj" },
      "Francie": { name: "Francie", info: "Paříž", population: "68,6 milionů obyvatel", rozloha: "643 801 km²", reky: "Loira, Rhôna" },
      "Itálie": { name: "Itálie", info: "Řím", population: "58,7 milionů obyvatel", rozloha: "301 338 km²", reky: "Pád, Adiže" },
          "Norsko": { name: "Norsko", info: "Oslo", population: "5,5 milionu obyvatel", rozloha: "385 207 km²", reky: "Glomma, Numedalslågen" },
  
          //Afrika
      "Egypt": { name: "Egypt", info: "Káhira", population: "109,3 milionů obyvatel", rozloha: "1 010 408 km²", reky: "Nil" },
      "Nigérie": { name: "Nigérie", info: "Abuja", population: "206 milionů obyvatel", rozloha: "923 768 km²", reky: "Niger, Benue" },
          "Jihoafrická republika": { name: "Jihoafrická republika", info: "Pretorie, Bloemfontein a Kapské Město", population: "62 milionů obyvatel", rozloha: "1 219 912 km²", reky: "Limpopo, Orange" },
          "Keňa": { name: "Keňa", info: "Nairobi", population: "55,8 milionů obyvatel", rozloha: "580 367 km²", reky: "Tana, Athi-Galana-Sabaki" },
          "Tanzanie": { name: "Tanzanie", info: "Dodoma", population: "53,9 milionů obyvatel", rozloha: "945 090 km²", reky: "Ruvuma, Murray" },
  
          //SA
      "USA": { name: "USA", info: "Washington D.C.", population: "340,9 milionů obyvatel", rozloha: "9 833 517 km²", reky: "Mississippi, Colorado" },
      "Kanada": { name: "Kanada", info: "Ottawa", population: "39,5 milionů obyvatel", rozloha: "	9 985 000 km²", reky: "Svatý Vavřinec, Mackenzie" },
      "Mexiko": { name: "Mexiko", info: "Ciudad de México", population: "131 milionů obyvatel", rozloha: "1 964 375 km²", reky: "Rio Grande, Conchos" },
      "Kuba": { name: "Kuba", info: "Havana", population: "11,3 milionů obyvatel", rozloha: "109 886 km²", reky: "Río Cauto, Río Almendares" },
          "Dominikánská republika": { name: "Dominikánská republika", info: "Santo Domingo", population: "10,9 milionů obyvatel", rozloha: "48 730 km²", reky: "Río Yaque del Norte, Río Yaque del Sur" },
  
          //JA
      "Brazílie": { name: "Brazílie", info: "Brasília", population: "217 milionů obyvatel", rozloha: "8 510 418 km²", reky: "Amazonka, Río Negro" },
      "Argentina": { name: "Argentina", info: "Buenos Aires", population: "46 milionů obyvatel", rozloha: "2 780 400 km²", reky: "Paraguay, Bermejo" },
      "Kolumbie": { name: "Kolumbie", info: "Bogotá", population: "52,6 milionů obyvatel", rozloha: "1 141 748 km²", reky: "Río Magdalena, Rio Cauca" },
      "Chile": { name: "Chile", info: "Santiago de Chile", population: "19 milionů obyvatel", rozloha: "756 102 km²", reky: "Río Bío-Bío, Río Loa" },
          "Peru": { name: "Peru", info: "Lima", population: "34 milionů obyvatel", rozloha: "1 285 216 km²", reky: "Río Amazonas, Río Ucayali" },
  
          //Asie
      "Čína": { name: "Čína", info: "Peking", population: "1,4 miliardy obyvatel", rozloha: "9 596 960 km²", reky: "Jang-c’-ťiang, Río Huang He" },
      "Indie": { name: "Indie", info: "Nové Dillí", population: "1,4 miliardy obyvatel", rozloha: "3 287 263 km²", reky: "Ganga, Indus" },
      "Japonsko": { name: "Japonsko", info: "Tokio", population: "125,4 milionů obyvatel", rozloha: "377 974 km²", reky: "Río Shinano, Río Tone" },
      "Thajsko": { name: "Thajsko", info: "Bangkok", population: "71 milionů obyvatel", rozloha: "513 120 km²", reky: "Río Chao Phraya, Río Mekong" },
          "Vietnam": { name: "Vietnam", info: "Hanoj", population: "101 milionů obyvatel", rozloha: "331 690 km²", reky: "Río Mekong, Río Rojo" }
    };
  
    const continentMaps = {
      "Evropa": "https://upload.wikimedia.org/wikipedia/commons/d/de/Europe_orthographic_Caucasus_Urals_boundary.svg",
      "Afrika": "https://upload.wikimedia.org/wikipedia/commons/8/86/Africa_%28orthographic_projection%29.svg",
      "Asie": "https://upload.wikimedia.org/wikipedia/commons/8/80/Asia_%28orthographic_projection%29.svg",
      "Severní Amerika": "https://upload.wikimedia.org/wikipedia/commons/4/43/Location_North_America.svg",
      "Jižní Amerika": "https://upload.wikimedia.org/wikipedia/commons/0/0f/South_America_%28orthographic_projection%29.svg"
    };
  
    function loadFlags() {
      flags = continents[selectedContinent] || [];
      selectedCountry = null;
    }
  
    function selectCountry(flag) {
      selectedCountry = countries[flag.name];
    }
  </script>
  
  <main>
    <h1>Vyber kontinent</h1>
    <select bind:value={selectedContinent} on:change={loadFlags}>
      <option value="">-- Vyber kontinent --</option>
      {#each Object.keys(continents) as continent}
        <option value={continent}>{continent}</option>
      {/each}
    </select>
  
    {#if selectedContinent}
      <h2>Mapa kontinentu {selectedContinent}</h2>
      <img src={continentMaps[selectedContinent]} alt={`Mapa ${selectedContinent}`} class="continent-map" />
    {/if}
  
    {#if flags.length > 0}
      <h2>Vlajky států v {selectedContinent}</h2>
      <div class="flags">
        {#each flags as flag}
          <div class="flag" on:click={() => selectCountry(flag)}>
            <img src={flag.flagUrl} alt={flag.name} width="100" />
            <p>{flag.name}</p>
          </div>
        {/each}
      </div>
    {/if}
  
    {#if selectedCountry}
      <h2>Informace o zemi: {selectedCountry.name}</h2>
      <p><strong>Hlavní město:</strong> {selectedCountry.info}</p>
      <p><strong>Populace:</strong> {selectedCountry.population}</p>
      <p><strong>Rozloha:</strong> {selectedCountry.rozloha}</p>
      <p><strong>Řeky:</strong> {selectedCountry.reky}</p>
    {/if}
  </main>
  
  <style>
    main {
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 1rem;
    }
  
    .continent-map {
      width: 60%;
      max-width: 500px;
      height: auto;
      margin: 20px 0;
      border: 2px solid #ccc;
      border-radius: 5px;
    }
  
    .flags {
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
      gap: 20px;
      margin-top: 20px;
    }
  
    .flag {
      cursor: pointer;
      text-align: center;
    }
  
    .flag img {
      width: 120px;
      height: auto;
      border: 2px solid #ccc;
      border-radius: 5px;
    }
  
    .flag:hover img {
      border-color: #000;
    }
  </style>
  