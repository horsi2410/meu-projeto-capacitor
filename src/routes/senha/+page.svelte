<script>
  let senha = '';
  let tamanho = 10;
  let quantidade = 1;

  function gerarSenha() {
    const caracteres = {
      maiusculas: 'ABCDEFGHIJKLMNOPQRSTUVWXYZ',
      minusculas: 'abcdefghijklmnopqrstuvwxyz',
      simbolos: '!@#$%^&*()_+[]{}|;:,.<>?',
      digitos: '0123456789'
    };

    let caracteresDisponiveis = '';
    if (document.getElementById('maiusculas').checked) caracteresDisponiveis += caracteres.maiusculas;
    if (document.getElementById('minusculas').checked) caracteresDisponiveis += caracteres.minusculas;
    if (document.getElementById('simbolos').checked) caracteresDisponiveis += caracteres.simbolos;
    if (document.getElementById('digitos').checked) caracteresDisponiveis += caracteres.digitos;

    if (!caracteresDisponiveis) {
      alert('Selecione pelo menos um tipo de caractere!');
      return;
    }

    let senhasGeradas = '';
    let senhasArray = [];
    for (let j = 0; j < quantidade; j++) { 
      senha = '';
      for (let i = 0; i < tamanho; i++) {
        const index = Math.floor(Math.random() * caracteresDisponiveis.length);
        senha += caracteresDisponiveis[index];
      }
      senhasArray.push(senha); 
    }

    senhasArray.forEach((senha, index) => {
      senhasGeradas += `
        <div>
          <input type="text" value="${senha}" id="senha${index}" readonly />
          <button onclick="copiarSenha(${index})">Copiar</button>
        </div>
      `;
    });

    document.getElementById('resultado').innerHTML = senhasGeradas;
  }

  function copiarSenha(index) {
    const senhaInput = document.getElementById(`senha${index}`);
    senhaInput.select();
    document.execCommand('copy');
    alert('Senha copiada!');
  }
</script>

<main>
  <h1>Gerador de Senha</h1>
  <div id="resultado"></div> 
  
  <div>
    <label>Tamanho da senha: </label>
    <input type="range" min="1" max="20" value="10" on:input="{(e) => { tamanho = e.target.value; }}" />
    <span>{tamanho}</span>
  </div>

  <div>
    <label><input type="checkbox" id="maiusculas" checked /> Letras Maiúsculas</label><br>
    <label><input type="checkbox" id="minusculas" checked /> Letras Minúsculas</label><br>
    <label><input type="checkbox" id="simbolos" /> Símbolos</label><br>
    <label><input type="checkbox" id="digitos" /> Números</label>
  </div>


  <div>
    <label>Quantidade de senhas: </label>
    <input type="number" min="1" max="10" bind:value={quantidade} />
  </div>

  <button on:click={gerarSenha}>Gerar Senha</button>
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
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1)1);
  }

  h1 {
    font-size: 1.5em;
    margin-bottom: 20px;
  }

  textarea {
    width: 100%;
    margin-bottom: 10px;
    padding: 10px;
    border: 1px solid #e70808;
    border-radius: 5px;
    resize: none;
  }

  button {
    padding: 10px 20px;
    background-color: #09eb7a;
    color: rgb(223, 0, 0);
    border: none;
    border-radius: 5px;
    cursor: pointer;
    margin-top: 10px;
  }

  button:hover {
    background-color: #fc0000;
  }

  div {
    margin: 15px 0;
    text-align: left;
  }

  label {
    font-size: 0.9em;
  }

  input[type="text"] {
    width: 80%;
    padding: 5px;
    margin-right: 10px;
  }
</style>
