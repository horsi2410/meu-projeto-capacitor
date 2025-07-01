<script>
  import { onMount } from "svelte";

  let novaTarefa = "";
  let busca = "";
  let exibir = "0";
  let tarefas = [];

  function adicionarTarefa(event) {
    event.preventDefault();
    if (novaTarefa.trim()) {
      tarefas = [...tarefas, { id: Date.now(), nome: novaTarefa, status: 0 }];
      novaTarefa = "";
    }
  }

  function alltasksdone() {
    tarefas = tarefas.map((t) => ({ ...t, status: 1 }));
  }

  function alltasksundone() {
    tarefas = tarefas.map((t) => ({ ...t, status: 0 }));
  }

  function alterarStatus(id) {
    tarefas = tarefas.map((t) =>
      t.id === id ? { ...t, status: t.status === 0 ? 1 : 0 } : t
    );
  }

  function filtrarTarefas() {
    return tarefas
      .filter((t) => {
        if (exibir === "0") return t.status === 0;
        if (exibir === "1") return t.status === 1;
        return true;
      })
      .filter((t) => t.nome.toLowerCase().includes(busca.toLowerCase()));
  }

  $: tarefasPendentes = tarefas.filter((t) => t.status === 0);
  $: tarefasConcluidas = tarefas.filter((t) => t.status === 1);
</script>

<!-- Topo fixo -->
<header class="topo-fixo">
  <button class="icon-btn left" aria-label="Menu">
    &#9776;
  </button>
  <h1 class="titulo">Disp.Móv.</h1>
  <button class="icon-btn right" aria-label="Mais opções">
    &#8942;
  </button>
</header>

<!-- Fundo rosa -->
<main class="fundo-rosa">
  <section class="card">
    <!-- Formulário adicionar tarefa -->
    <form class="form-adicionar" on:submit={adicionarTarefa}>
      <input
        type="text"
        placeholder="Nova tarefa"
        bind:value={novaTarefa}
        aria-label="Nova tarefa"
      />
      <button type="submit" aria-label="Adicionar tarefa">+</button>
    </form>

    <!-- Busca e filtros -->
    <input
      type="text"
      placeholder="Busca"
      bind:value={busca}
      aria-label="Buscar tarefas"
      class="input-busca"
    />
    <select bind:value={exibir} aria-label="Filtrar tarefas" class="select-filtro">
      <option value="0">Pendentes</option>
      <option value="1">Concluídas</option>
      <option value="2">Todas</option>
    </select>

    <div class="botoes-acoes">
      <button type="button" on:click={alltasksdone} class="btn-verde">
        Todas Concluídas
      </button>
      <button type="button" on:click={alltasksundone} class="btn-amarelo">
        Todas Pendentes
      </button>
    </div>

    <div class="contadores">
      <span>Total: {tarefas.length}</span>
      <span>Pendentes: {tarefasPendentes.length}</span>
      <span>Concluídas: {tarefasConcluidas.length}</span>
    </div>

    <!-- Lista de tarefas -->
    <ul class="lista-tarefas">
      {#each filtrarTarefas() as tarefa (tarefa.id)}
        <li
          class:concluida={tarefa.status === 1}
          on:click={() => alterarStatus(tarefa.id)}
          tabindex="0"
          role="button"
          aria-pressed={tarefa.status === 1}
        >
          {tarefa.nome}
        </li>
      {/each}
    </ul>
  </section>
</main>

<style>
  /* RESET simples */
  * {
    box-sizing: border-box;
  }
  body, html {
    margin: 0;
    padding: 0;
    font-family: system-ui, sans-serif;
    background: #f0f4ff;
  }

  /* TOPO FIXO */
  .topo-fixo {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 50px;
    background-color: #3b82f6; /* azul forte */
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 12px;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.15);
    color: white;
    z-index: 1000;
  }

  .icon-btn {
    font-size: 24px;
    background: none;
    border: none;
    color: white;
    cursor: pointer;
    user-select: none;
  }
  .icon-btn:focus {
    outline: 2px solid #fff;
    outline-offset: 2px;
  }

  .titulo {
    font-weight: 600;
    font-size: 18px;
    user-select: none;
  }

  /* FUNDO ROSA */
  .fundo-rosa {
    padding: 70px 12px 20px; /* espaço para topo fixo */
    min-height: 100vh;
    background: linear-gradient(180deg, #f9d5e5 0%, #e0c6f5 100%);
    display: flex;
    justify-content: center;
  }

  /* CARD */
  .card {
    background: white;
    border-radius: 16px;
    max-width: 370px;
    width: 100%;
    padding: 20px;
    box-shadow: 0 8px 25px rgba(0,0,0,0.12);
    display: flex;
    flex-direction: column;
    gap: 14px;
  }

  /* FORMULARIO ADICIONAR */
  .form-adicionar {
    display: flex;
    gap: 8px;
  }
  .form-adicionar input {
    flex-grow: 1;
    padding: 10px 14px;
    font-size: 1.1rem;
    border: 1px solid #ccc;
    border-radius: 12px;
    outline: none;
    transition: border-color 0.3s;
  }
  .form-adicionar input:focus {
    border-color: #3b82f6;
  }
  .form-adicionar button {
    background-color: #3b82f6;
    color: white;
    border: none;
    border-radius: 12px;
    font-size: 1.5rem;
    width: 45px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  .form-adicionar button:hover {
    background-color: #2563eb;
  }

  /* BUSCA E FILTRO */
  .input-busca {
    padding: 10px 14px;
    font-size: 1.05rem;
    border-radius: 12px;
    border: 1px solid #ccc;
    outline: none;
    transition: border-color 0.3s;
  }
  .input-busca:focus {
    border-color: #3b82f6;
  }

  .select-filtro {
    margin-top: 6px;
    padding: 10px 14px;
    font-size: 1rem;
    border-radius: 12px;
    border: 1px solid #ccc;
    outline: none;
    width: 100%;
    transition: border-color 0.3s;
  }
  .select-filtro:focus {
    border-color: #3b82f6;
  }

  /* BOTOES AÇÕES */
  .botoes-acoes {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin-top: 12px;
  }
  .botoes-acoes button {
    padding: 12px;
    font-size: 1rem;
    border: none;
    border-radius: 12px;
    cursor: pointer;
    color: white;
    transition: background-color 0.3s;
  }
  .btn-verde {
    background-color: #22c55e;
  }
  .btn-verde:hover {
    background-color: #16a34a;
  }
  .btn-amarelo {
    background-color: #eab308;
  }
  .btn-amarelo:hover {
    background-color: #ca8a04;
  }

  /* CONTADORES */
  .contadores {
    margin-top: 12px;
    font-size: 0.9rem;
    color: #555;
    display: flex;
    flex-direction: column;
    gap: 4px;
  }

  /* LISTA DE TAREFAS */
  .lista-tarefas {
    margin-top: 14px;
    list-style: none;
    padding: 0;
    max-height: 220px;
    overflow-y: auto;
    border-top: 1px solid #ddd;
  }
  .lista-tarefas li {
    padding: 10px 14px;
    border-bottom: 1px solid #eee;
    cursor: pointer;
    user-select: none;
    transition: background-color 0.2s;
  }
  .lista-tarefas li:hover,
  .lista-tarefas li:focus {
    background-color: #f0f9ff;
    outline: none;
  }
  .lista-tarefas li.concluida {
    color: #999;
    text-decoration: line-through;
  }

  /* SCROLL customizado */
  .lista-tarefas::-webkit-scrollbar {
    width: 6px;
  }
  .lista-tarefas::-webkit-scrollbar-thumb {
    background-color: rgba(59, 130, 246, 0.5);
    border-radius: 3px;
  }

  /* RESPONSIVO: */
  @media (max-width: 380px) {
    .card {
      max-width: 370px;
      padding: 16px;
    }
    .form-adicionar input {
      font-size: 1rem;
      padding: 8px 12px;
    }
    .form-adicionar button {
      font-size: 1.3rem;
      width: 40px;
    }
    .input-busca {
      font-size: 1rem;
      padding: 8px 12px;
    }
    .select-filtro {
      font-size: 0.95rem;
      padding: 8px 12px;
    }
    .botoes-acoes button {
      font-size: 0.95rem;
      padding: 10px;
    }
    .contadores {
      font-size: 0.85rem;
    }
    .lista-tarefas li {
      font-size: 0.95rem;
      padding: 8px 12px;
    }
  }
</style>
