<script>
    import { page } from '$app/stores';
    import { dicionario } from '$lib/dicionario';
  
    const palavra = $page.params.palavra.toLowerCase();
    const termo = dicionario.find(item => item.palavra.toLowerCase() === palavra);
  </script>
  
  <style>
    h1 {
      font-size: 1.5rem;
      font-weight: bold;
      color: #2b6cb0; /* Azul */
      margin-bottom: 1rem;
    }
  
    ul {
      list-style: none;
      padding: 0;
    }
  
    li {
      margin: 0.5rem 0;
      font-size: 1rem;
      color: #4a5568; /* Cinza escuro */
    }
  
    p {
      color: #e53e3e; /* Vermelho */
      font-size: 1.2rem;
      margin-top: 1rem;
    }
  
    a {
      color: #3182ce; /* Azul */
      text-decoration: underline;
      margin-top: 1rem;
      display: inline-block;
    }
  
    a:hover {
      color: #2c5282;
    }
  
    div {
      margin: 1rem;
      padding: 1rem;
      border: 1px solid #cbd5e0; /* Cinza claro */
      border-radius: 8px;
      background: #f7fafc; 
    }
  </style>
  
  {#if termo}
    <div>
      <h1>{termo.palavra}</h1>
      <ol>
        {#each termo.definicoes as definicao}
          <li>{definicao}</li>
        {/each}
      </ol>
      <a href="/dicionario">Voltar</a>
    </div>
  {:else}
    <div>
      <p>Palavra não encontrada!</p>
      <a href="/dicionario">Voltar</a>
    </div>
  {/if}
  