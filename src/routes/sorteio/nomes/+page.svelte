<script>
    let nomes = [];
    let novoNome = '';
    let resultado = '';
  
    function adicionarNome() {
      if (novoNome.trim()) {
        nomes = [...nomes, novoNome];
        novoNome = '';
      }
    }
  
    function sortear() {
      if (nomes.length > 0) {
        const index = Math.floor(Math.random() * nomes.length);
        resultado = `Sorteado: ${nomes[index]}`;
      } else {
        resultado = 'Adicione nomes!';
      }
    }
  </script>
  
  <div class="container">
    <h1>Sorteio de nomes</h1>
    <input
      type="text"
      bind:value={novoNome}
      placeholder="Digite um nome"
    />
    <button on:click={adicionarNome}>Adicionar</button>
  
    <ul>
      {#each nomes as nome}
        <li>{nome}</li>
      {/each}
    </ul>
  
    <button class="sortear" on:click={sortear}>Sortear</button>
  
    {#if resultado}
      <p class="resultado">{resultado}</p>
    {/if}
  
    <h1 class="nav-item">
      <a class="nav-link active" href="/sorteio">Voltar</a>
    </h1>
  </div>
  
  <style>
    .container {
      padding: 20px;
      text-align: center;
      font-family: Arial, sans-serif;
      max-width: 400px;
      margin: auto;
    }
  
    h1 {
      font-size: 1.5em;
      margin-bottom: 15px;
      color: #333;
    }
  
    input[type="text"] {
      width: 90%;
      padding: 10px;
      margin-bottom: 15px;
      font-size: 1em;
      border: 1px solid #ccc;
      border-radius: 8px;
      box-sizing: border-box;
    }
  
    button {
      width: 90%;
      padding: 10px;
      margin-bottom: 15px;
      font-size: 1em;
      color: white;
      background-color: #007BFF;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }
  
    button:hover {
      background-color: #0056b3;
    }
  
    .sortear {
      background-color: #007BFF;
    }
  
    ul {
      list-style: none;
      padding: 0;
      text-align: left;
      margin-bottom: 20px;
      max-height: 200px;
      overflow-y: auto;
      border: 1px solid #ddd;
      border-radius: 8px;
      background-color: #f0f0f0;
    }
  
    ul li {
      margin: 5px 10px;
      padding: 5px 10px;
      border-radius: 5px;
      background-color: white;
      color: #333;
    }
  
    .resultado {
      margin-top: 20px;
      font-size: 1.2em;
      font-weight: bold;
      color: #007BFF;
    }
  
    .nav-item {
      margin-top: 30px;
    }
  
    .nav-link {
      display: inline-block;
      text-decoration: none;
      color: white;
      background-color: #4CAF50;
      padding: 10px 20px;
      border-radius: 8px;
      font-size: 1.1em;
      transition: background-color 0.3s ease;
    }
  
    .nav-link:hover {
      background-color: #45a049;
    }
  
    .nav-link.active {
      background-color: #45a049;
    }
  </style>
  