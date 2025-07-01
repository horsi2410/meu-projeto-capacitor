<script>
  // Objeto com as categorias de conversão e suas opções correspondentes
  const tiposDeConversao = {
    "Temperatura": [
      { nome: "Graus Celsius → Fahrenheit", de: "°C", para: "°F", converter: (val) => (val * 9/5) + 32, inverter: (val) => (val - 32) * 5/9 },
      { nome: "Fahrenheit → Graus Celsius", de: "°F", para: "°C", converter: (val) => (val - 32) * 5/9, inverter: (val) => (val * 9/5) + 32 }
    ],
    "Peso": [
      { nome: "Quilos → Gramas", de: "kg", para: "g", converter: (val) => val * 1000, inverter: (val) => val / 1000 },
      { nome: "Gramas → Quilos", de: "g", para: "kg", converter: (val) => val / 1000, inverter: (val) => val * 1000 },
      { nome: "Quilos → Libras", de: "kg", para: "lb", converter: (val) => val * 2.20462, inverter: (val) => val / 2.20462 },
      { nome: "Libras → Quilos", de: "lb", para: "kg", converter: (val) => val / 2.20462, inverter: (val) => val * 2.20462 },
    ],
    "Comprimento": [
      { nome: "Metros → Centímetros", de: "m", para: "cm", converter: (val) => val * 100, inverter: (val) => val / 100 },
      { nome: "Centímetros → Metros", de: "cm", para: "m", converter: (val) => val / 100, inverter: (val) => val * 100 },
      { nome: "Metros → Jardas", de: "m", para: "yd", converter: (val) => val * 1.09361, inverter: (val) => val / 1.09361 },
      { nome: "Quilômetros → Milhas", de: "km", para: "mi", converter: (val) => val * 0.621371, inverter: (val) => val / 0.621371 },
      { nome: "Milhas → Quilômetros", de: "mi", para: "km", converter: (val) => val / 0.621371, inverter: (val) => val * 0.621371 }
    ],
    "Área": [
      { nome: "Metros quadrados → Centímetros quadrados", de: "m²", para: "cm²", converter: (val) => val * 10000, inverter: (val) => val / 10000 },
      { nome: "Centímetros quadrados → Metros quadrados", de: "cm²", para: "m²", converter: (val) => val / 10000, inverter: (val) => val * 10000 },
      { nome: "Quilômetros quadrados → Metros quadrados", de: "km²", para: "m²", converter: (val) => val * 1000000, inverter: (val) => val / 1000000 },
    ],
    "Volume": [
      { nome: "Litros → Mililitros", de: "L", para: "mL", converter: (val) => val * 1000, inverter: (val) => val / 1000 },
      { nome: "Mililitros → Litros", de: "mL", para: "L", converter: (val) => val / 1000, inverter: (val) => val * 1000 },
    ],
    "Velocidade": [
      { nome: "Quilômetros por hora → Metros por segundo", de: "km/h", para: "m/s", converter: (val) => val / 3.6, inverter: (val) => val * 3.6 },
      { nome: "Metros por segundo → Quilômetros por hora", de: "m/s", para: "km/h", converter: (val) => val * 3.6, inverter: (val) => val / 3.6 },
      { nome: "Quilômetros por hora → Milhas por hora", de: "km/h", para: "mph", converter: (val) => val / 1.60934, inverter: (val) => val * 1.60934 }
    ],
    "Tempo": [
      { nome: "Segundos → Minutos", de: "s", para: "min", converter: (val) => val / 60, inverter: (val) => val * 60 },
      { nome: "Minutos → Segundos", de: "min", para: "s", converter: (val) => val * 60, inverter: (val) => val / 60 },
      { nome: "Horas → Minutos", de: "h", para: "min", converter: (val) => val * 60, inverter: (val) => val / 60 },
      { nome: "Minutos → Horas", de: "min", para: "h", converter: (val) => val / 60, inverter: (val) => val * 60 },
      { nome: "Dias → Horas", de: "d", para: "h", converter: (val) => val * 24, inverter: (val) => val / 24 }
    ]
  };

  // Categoria selecionada, padrão "Temperatura"
  let categoriaSelecionada = "Temperatura";
  // Conversão selecionada, padrão a primeira da categoria
  let conversaoSelecionada = tiposDeConversao["Temperatura"][0];
  // Valor digitado para converter
  let valorEntrada = "";
  // Resultado da conversão
  let valorResultado = "";
  // Hora atual formatada
  let horaAtual = obterHoraAtual();
  // Flag para indicar se está invertido
  let estaInvertido = false;

  // Função para obter hora atual no formato 12h com AM/PM
  function obterHoraAtual() {
    const agora = new Date();
    let horas = agora.getHours();
    const minutos = agora.getMinutes().toString().padStart(2, '0');
    const ampm = horas >= 12 ? 'PM' : 'AM';
    
    horas = horas % 12;
    horas = horas ? horas : 12;
    
    return `${horas}:${minutos} ${ampm}`;
  }

  // Troca categoria e reseta conversão e valores
  function selecionarCategoria(categoria) {
    categoriaSelecionada = categoria;
    conversaoSelecionada = tiposDeConversao[categoria][0];
    valorEntrada = "";
    valorResultado = "";
    estaInvertido = false;
  }

  // Troca conversão e reseta valores
  function selecionarConversao(conversao) {
    conversaoSelecionada = conversao;
    valorEntrada = "";
    valorResultado = "";
    estaInvertido = false;
  }

  // Calcula conversão baseado no valor, conversão e direção
  function calcular() {
    if (!conversaoSelecionada || valorEntrada === "" || isNaN(valorEntrada)) {
      valorResultado = "";
      return;
    }
    
    const valor = parseFloat(valorEntrada);
    let convertido;
    
    if (estaInvertido) {
      convertido = conversaoSelecionada.inverter(valor);
    } else {
      convertido = conversaoSelecionada.converter(valor);
    }
    
    if (Number.isInteger(convertido)) {
      valorResultado = convertido.toString();
    } else {
      valorResultado = convertido.toFixed(2).replace(/\.?0+$/, '');
    }
  }

  // Inverte sentido da conversão e troca valores de entrada e resultado
  function inverterConversao() {
    if (!conversaoSelecionada || valorEntrada === "" || isNaN(valorEntrada)) return;
    
    estaInvertido = !estaInvertido;
  
    const temp = valorEntrada;
    valorEntrada = valorResultado;
    valorResultado = temp;
    
    if (estaInvertido) {
      conversaoSelecionada = {
        ...conversaoSelecionada,
        nome: `${conversaoSelecionada.para} → ${conversaoSelecionada.de}`,
        de: conversaoSelecionada.para,
        para: conversaoSelecionada.de
      };
    } else {
      const original = tiposDeConversao[categoriaSelecionada].find(c =>
        c.de === conversaoSelecionada.para && c.para === conversaoSelecionada.de
      );
      if (original) conversaoSelecionada = original;
    }
  }

  // Atualiza hora a cada minuto
  setInterval(() => {
    horaAtual = obterHoraAtual();
  }, 60000);
</script>

<!-- Interface do conversor -->
<div class="app-conversor">
  <header>
    <div class="exibe-hora">{horaAtual}</div>
    <h1>Conversor</h1>
    <p class="subtitulo">Escolha o que deseja converter</p>
  </header>
  
  <main>
    <div class="conversor-mobile">
      <!-- Seletor de categoria -->
      <div class="seletor-categoria">
        <select bind:value={categoriaSelecionada} on:change={() => selecionarCategoria(categoriaSelecionada)}>
          {#each Object.keys(tiposDeConversao) as categoria}
            <option value={categoria}>{categoria}</option>
          {/each}
        </select>
      </div>
      
      <!-- Seletor de conversão -->
      <div class="seletor-conversao">
        <select bind:value={conversaoSelecionada} on:change={() => selecionarConversao(conversaoSelecionada)}>
          {#each tiposDeConversao[categoriaSelecionada] as conversao}
            <option value={conversao}>{conversao.nome}</option>
          {/each}
        </select>
      </div>
      
      <!-- Área de entrada e resultado -->
      <div class="area-conversao">
        <div class="entrada-valor">
          <label>Valor em {conversaoSelecionada?.de}:</label>
          <input
            type="number"
            bind:value={valorEntrada}
            on:input={calcular}
            placeholder="Digite o valor"
          />
        </div>
        
        <div class="resultado-valor">
          <label>Resultado em {conversaoSelecionada?.para}:</label>
          <div class="resultado">
            {valorResultado || "---"}
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

  .app-conversor {
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

  .exibe-hora {
    font-size: 1rem;
    color: #f5f5f5;
  }

  h1 {
    font-size: 2.5rem;
    margin: 10px 0;
    color: #ffffff;
    font-weight: bold;
  }

  .subtitulo {
    color: #ffffff;
    font-size: 1.1rem;
  }

  .conversor-mobile {
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

  .area-conversao {
    background-color: #ffffff;
    padding: 20px;
    border-radius: 15px;
    box-shadow: 0 4px 15px rgba(184, 223, 255, 0.1);
    flex-grow: 1;
  }

  .entrada-valor, .resultado-valor {
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

  .resultado {
    font-size: 1.5rem;
    font-weight: bold;
    color: #333;
    margin-top: 10px;
  }

  @media (max-width: 768px) {
    .app-conversor {
      padding: 10px;
    }
  }
</style>
