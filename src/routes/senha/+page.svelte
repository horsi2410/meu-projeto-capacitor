<script>
  let tamanho = 10;
  let quantidade = 1;
  let senhas = [];

  const caracteres = {
    maiusculas: 'ABCDEFGHIJKLMNOPQRSTUVWXYZ',
    minusculas: 'abcdefghijklmnopqrstuvwxyz',
    simbolos: '!@#$%^&*()_+[]{}|;:,.<>?',
    digitos: '0123456789',
  };

  function gerarSenha() {
    let caracteresDisponiveis = '';
    if (document.getElementById('maiusculas').checked) caracteresDisponiveis += caracteres.maiusculas;
    if (document.getElementById('minusculas').checked) caracteresDisponiveis += caracteres.minusculas;
    if (document.getElementById('simbolos').checked) caracteresDisponiveis += caracteres.simbolos;
    if (document.getElementById('digitos').checked) caracteresDisponiveis += caracteres.digitos;

    if (!caracteresDisponiveis) {
      alert('Selecione pelo menos um tipo de caractere!');
      return;
    }

    senhas = [];

    for (let j = 0; j < quantidade; j++) {
      let senha = '';
      for (let i = 0; i < tamanho; i++) {
        const index = Math.floor(Math.random() * caracteresDisponiveis.length);
        senha += caracteresDisponiveis[index];
      }
      senhas.push(senha);
    }
  }

  async function copiarSenha(texto) {
    try {
      await navigator.clipboard.writeText(texto);
      alert('Senha copiada!');
    } catch (err) {
      alert('Erro ao copiar senha: ' + err);
    }
  }
</script>

<main>
  <h1>Gerador de Senha</h1>

  <div>
    <label>Tamanho da senha: </label>
    <input
      type="range"
      min="1"
      max="20"
      bind:value={tamanho}
    />
    <span>{tamanho}</span>
  </div>

  <div>
    <label><input type="checkbox" id="maiusculas" checked /> Letras Maiúsculas</label><br />
    <label><input type="checkbox" id="minusculas" checked /> Letras Minúsculas</label><br />
    <label><input type="checkbox" id="simbolos" /> Símbolos</label><br />
    <label><input type="checkbox" id="digitos" /> Números</label>
  </div>

  <div>
    <label>Quantidade de senhas: </label>
    <input type="number" min="1" max="10" bind:value={quantidade} />
  </div>

  <button on:click={gerarSenha}>Gerar Senha</button>

  <div id="resultado" style="margin-top: 20px;">
    {#each senhas as senha, index}
      <div style="margin-bottom: 10px;">
        <input type="text" readonly value={senha} style="width: 80%; padding: 5px; margin-right: 10px; border-radius: 5px; border: 1px solid #ccc;" />
        <button on:click={() => copiarSenha(senha)} style="padding: 5px 10px; border-radius: 5px; background-color: #007bff; color: white; border: none; cursor: pointer;">
          Copiar
        </button>
      </div>
    {/each}
  </div>
</main>

<style>
  main {
    text-align: center;
    padding: 20px;
    font-family: Arial, sans-serif;
    background-image: url('Brasil-x-Argentina-07-Adriano-comemora-1578x1920.webp');
    background-size: cover;
    background-position: center;
    max-width: 400px;
    margin: auto;
    border-radius: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    color: white;
  }

  h1 {
    font-size: 1.5em;
    margin-bottom: 20px;
    text-shadow: 1px 1px 3px #000;
  }

  div {
    margin: 15px 0;
    text-align: left;
  }

  label {
    font-size: 0.9em;
  }

  input[type='range'] {
    width: 70%;
  }

  input[type='number'] {
    width: 60px;
    padding: 5px;
    border-radius: 5px;
    border: 1px solid #ccc;
  }

  button {
    padding: 10px 20px;
    background-color: #09eb7a;
    color: rgb(223, 0, 0);
    border: none;
    border-radius: 5px;
    cursor: pointer;
    margin-top: 10px;
    transition: background-color 0.3s ease;
  }

  button:hover {
    background-color: #fc0000;
    color: white;
  }
</style>