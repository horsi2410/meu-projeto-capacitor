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
        resultado = 'Adicione itens!';
      }
    }
  </script>
  
  <div style="padding: 20px; text-align: center; font-family: Arial, sans-serif;">
    <h1 style="font-size: 1.5em; margin-bottom: 10px;">Sorteio de frutas</h1>
    <input
      type="text"
      bind:value={novoNome}
      placeholder="Digite uma fruta"
      style="width: 90%; padding: 10px; margin-bottom: 10px;"
    />
    <button on:click={adicionarNome} style="width: 90%; padding: 10px; margin-bottom: 10px;">Adicionar</button>
    <ul style="list-style: none; padding: 0; text-align: left; margin-bottom: 20px;">
      {#each nomes as nome}
        <li style="margin: 5px 0; padding: 5px; background: #f0f0f0; border-radius: 5px;">{nome}</li>
      {/each}
    </ul>
    <button on:click={sortear} style="width: 90%; padding: 10px; background: #007BFF; color: white;">Sortear</button>
    {#if resultado}
      <p style="margin-top: 20px; font-size: 1.2em; font-weight: bold;">{resultado}</p>
    {/if}
    <br>
<h1 class="nav-item"><a class="nav-link active" href="/sorteio">Voltar</a></h1>

  </div>
  