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
        resultado = 'Adicione algo!';
      }
    }
  </script>
  
  <div>
    <h1>Sorteio Aleatório</h1>
    <input type="text" bind:value={novoNome} placeholder="Digite algo" />
    <button on:click={adicionarNome}>Adicionar</button>
    <ul>
      {#each nomes as nome}
        <li>{nome}</li>
      {/each}
    </ul>
    <button on:click={sortear}>Sortear</button>
    {#if resultado}
      <p class="resultado">{resultado}</p>
    {/if}
  
    <br><br>
    <h2>Caso está sem criatividade</h2>
    <div class="small-buttons">
      <a class="nav-link" href="/senha">Clique aqui</a>
      <a class="nav-link" href="/sorteio">Voltar</a>
    </div>
  </div>
  
  <style>
    h1, h2 {
      font-size: 1.5em;
      margin-bottom: 10px;
      color: #333;
      text-align: center;
    }
  
    input {
      width: 90%;
      padding: 10px;
      margin-bottom: 10px;
      border: 1px solid #ccc;
      border-radius: 8px;
      font-size: 1em;
    }
  
    button {
      width: 90%;
      padding: 10px;
      margin-bottom: 10px;
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
  
    ul {
      list-style: none;
      padding: 0;
      text-align: left;
      margin-bottom: 20px;
    }
  
    li {
      margin: 5px 0;
      padding: 5px;
      background: #f0f0f0;
      border-radius: 5px;
    }
  
    .nav-link {
      text-decoration: none;
      padding: 5px 15px;
      color: rgb(255, 255, 255);
      background-color: #2c80ff;
      border-radius: 5px;
      font-size: 0.9em;
      margin: 5px;
      display: inline-block;
    }
  
    .resultado {
      margin-top: 20px;
      font-size: 1.2em;
      font-weight: bold;
      text-align: center;
    }
  
    .small-buttons {
      text-align: center;
    }
  
    .small-buttons .nav-link {
      margin: 5px;
    }
  </style>
  