<script>
  const conversionTypes = {
    "Temperatura": [
      { name: "Graus Celsius → Fahrenheit", from: "°C", to: "°F", convert: (val) => (val * 9/5) + 32, reverse: (val) => (val - 32) * 5/9 },
      { name: "Fahrenheit → Graus Celsius", from: "°F", to: "°C", convert: (val) => (val - 32) * 5/9, reverse: (val) => (val * 9/5) + 32 }
    ],
    "Peso": [
      { name: "Quilos → Gramas", from: "kg", to: "g", convert: (val) => val * 1000, reverse: (val) => val / 1000 },
      { name: "Gramas → Quilos", from: "g", to: "kg", convert: (val) => val / 1000, reverse: (val) => val * 1000 },
      { name: "Quilos → Libras", from: "kg", to: "lb", convert: (val) => val * 2.20462, reverse: (val) => val / 2.20462 },
      { name: "Libras → Quilos", from: "lb", to: "kg", convert: (val) => val / 2.20462, reverse: (val) => val * 2.20462 },
    ],
    "Comprimento": [
      { name: "Metros → Centímetros", from: "m", to: "cm", convert: (val) => val * 100, reverse: (val) => val / 100 },
      { name: "Centímetros → Metros", from: "cm", to: "m", convert: (val) => val / 100, reverse: (val) => val * 100 },
      { name: "Metros → Jardas", from: "m", to: "yd", convert: (val) => val * 1.09361, reverse: (val) => val / 1.09361 },
      { name: "Quilômetros → Milhas", from: "km", to: "mi", convert: (val) => val * 0.621371, reverse: (val) => val / 0.621371 },
     { name: "Milhas → Quilômetros", from: "mi", to: "km", convert: (val) => val / 0.621371, reverse: (val) => val * 0.621371 }
    ],
    "Área": [
      { name: "Metros quadrados → Centímetros quadrados", from: "m²", to: "cm²", convert: (val) => val * 10000, reverse: (val) => val / 10000 },
      { name: "Centímetros quadrados → Metros quadrados", from: "cm²", to: "m²", convert: (val) => val / 10000, reverse: (val) => val * 10000 },
      { name: "Quilômetros quadrados → Metros quadrados", from: "km²", to: "m²", convert: (val) => val * 1000000, reverse: (val) => val / 1000000 },
    ],
    "Volume": [
      { name: "Litros → Mililitros", from: "L", to: "mL", convert: (val) => val * 1000, reverse: (val) => val / 1000 },
      { name: "Mililitros → Litros", from: "mL", to: "L", convert: (val) => val / 1000, reverse: (val) => val * 1000 },
    ],
    "Velocidade": [
      { name: "Quilômetros por hora → Metros por segundo", from: "km/h", to: "m/s", convert: (val) => val / 3.6, reverse: (val) => val * 3.6 },
      { name: "Metros por segundo → Quilômetros por hora", from: "m/s", to: "km/h", convert: (val) => val * 3.6, reverse: (val) => val / 3.6 },
      { name: "Quilômetros por hora → Milhas por hora", from: "km/h", to: "mph", convert: (val) => val / 1.60934, reverse: (val) => val * 1.60934 }
    ],
    "Tempo": [
      { name: "Segundos → Minutos", from: "s", to: "min", convert: (val) => val / 60, reverse: (val) => val * 60 },
      { name: "Minutos → Segundos", from: "min", to: "s", convert: (val) => val * 60, reverse: (val) => val / 60 },
      { name: "Horas → Minutos", from: "h", to: "min", convert: (val) => val * 60, reverse: (val) => val / 60 },
      { name: "Minutos → Horas", from: "min", to: "h", convert: (val) => val / 60, reverse: (val) => val * 60 },
      { name: "Dias → Horas", from: "d", to: "h", convert: (val) => val * 24, reverse: (val) => val / 24 }
    ]
  };

  let selectedCategory = "Temperatura";
  let selectedConversion = conversionTypes["Temperatura"][0];
  let inputValue = "";
  let resultValue = "";
  let currentTime = getCurrentTime();
  let isReversed = false;

  function getCurrentTime() {
    const now = new Date();
    let hours = now.getHours();
    const minutes = now.getMinutes().toString().padStart(2, '0');
    const ampm = hours >= 12 ? 'PM' : 'AM';
    
    hours = hours % 12;
    hours = hours ? hours : 12;
    
    return `${hours}:${minutes} ${ampm}`;
  }

  function selectCategory(category) {
    selectedCategory = category;
    selectedConversion = conversionTypes[category][0];
    inputValue = "";
    resultValue = "";
    isReversed = false;
  }

  function selectConversion(conversion) {
    selectedConversion = conversion;
    inputValue = "";
    resultValue = "";
    isReversed = false;
  }

  function calculate() {
    if (!selectedConversion || inputValue === "" || isNaN(inputValue)) {
      resultValue = "";
      return;
    }
    
    const value = parseFloat(inputValue);
    let converted;
    
    if (isReversed) {
      converted = selectedConversion.reverse(value);
    } else {
      converted = selectedConversion.convert(value);
    }
    
    if (Number.isInteger(converted)) {
      resultValue = converted.toString();
    } else {
      resultValue = converted.toFixed(2).replace(/\.?0+$/, '');
    }
  }

  function toggleConversion() {
    if (!selectedConversion || inputValue === "" || isNaN(inputValue)) return;
    
    isReversed = !isReversed;
  
    const temp = inputValue;
    inputValue = resultValue;
    resultValue = temp;
    
    if (isReversed) {
      selectedConversion = {
        ...selectedConversion,
        name: `${selectedConversion.to} → ${selectedConversion.from}`,
        from: selectedConversion.to,
        to: selectedConversion.from
      };
    } else {
      const original = conversionTypes[selectedCategory].find(c => 
        c.from === selectedConversion.to && c.to === selectedConversion.from
      );
      if (original) selectedConversion = original;
    }
  }

  setInterval(() => {
    currentTime = getCurrentTime();
  }, 60000);
</script>

<div class="converter-app">
  <header>
    <div class="time-display">{currentTime}</div>
    <h1>Conversor</h1>
    <p class="subtitle">Selecione o que deseja converter</p>
  </header>
  
  <main>
    <div class="mobile-converter">
      <div class="category-selector">
        <select bind:value={selectedCategory} on:change={() => selectCategory(selectedCategory)}>
          {#each Object.keys(conversionTypes) as category}
            <option value={category}>{category}</option>
          {/each}
        </select>
      </div>
      
      <div class="conversion-selector">
        <select bind:value={selectedConversion} on:change={() => selectConversion(selectedConversion)}>
          {#each conversionTypes[selectedCategory] as conversion}
            <option value={conversion}>{conversion.name}</option>
          {/each}
        </select>
      </div>
      
      <div class="conversion-area">
        <div class="input-section">
          <label>Valor em {selectedConversion?.from}:</label>
          <input 
            type="number" 
            bind:value={inputValue}
            on:input={calculate}
            placeholder="Digite o valor"
          />
        </div>
        
        <div class="result-section">
          <label>Resultado em {selectedConversion?.to}:</label>
          <div class="result">
            {resultValue || "---"}
          </div>
        </div>


      </div>
    </div>
  </main>
</div>

<style>
   :global(body) {
    margin: 0;
    padding: 0;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: #74b4da;
    height: 100vh;
  }

  .converter-app {
    max-width: 100%;
    margin: 0 auto;
    padding: 20px;
    display: flex;
    flex-direction: column;
    height: 100%;
  }

  header {
    text-align: center;
    margin-bottom: 20px;
  }

  .time-display {
    font-size: 1rem;
    color: #f5f5f5;
  }

  h1 {
    font-size: 2.5rem;
    margin: 10px 0;
    color: #ffffff;
    font-weight: bold;
  }

  .subtitle {
    color: #ffffff;
    font-size: 1.1rem;
  }

  .mobile-converter {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  select {
    width: 100%;
    padding: 15px;
    border: 2px solid #000000;
    border-radius: 10px;
    background-color: #fff;
    font-size: 1rem;
    margin-bottom: 10px;
  }

  .conversion-area {
    background-color: #ffffff;
    padding: 20px;
    border-radius: 15px;
    box-shadow: 0 4px 15px rgba(184, 223, 255, 0.1);
    flex-grow: 1;
  }

  .input-section, .result-section {
    margin-bottom: 20px;
  }

  label {
    display: block;
    font-size: 1rem;
    color: #000000;
    margin-bottom: 10px;
  }

  input {
    width: 100%;
    padding: 15px;
    border: 2px solid #122e69;
    border-radius: 10px;
    font-size: 1.1rem;
    box-sizing: border-box;
  }

  input:focus {
    border-color: #ffffff;
    outline: none;
  }

  .calculate-btn {
    padding: 15px;
    background-color: #e3f4ff;
    color: rgba(0, 0, 0, 0.89);
    border: none;
    border-radius: 10px;
    font-size: 1.1rem;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  .calculate-btn:hover {
    background-color: #1d90dd;
  }

  .result {
    font-size: 1.5rem;
    font-weight: bold;
    color: #333;
    margin-top: 10px;
  }

  .icon {
    margin-right: 10px;
  }
  
  @media (max-width: 768px) {
    .converter-app {
      padding: 10px;
    }

    .calculate-btn {
      font-size: 1rem;
    }
  }
</style>
