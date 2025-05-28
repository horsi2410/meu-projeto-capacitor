<script>
  import { onMount } from 'svelte';
  import Modal from '$lib/components/Modal.svelte';
  import Toast from '$lib/components/Toast.svelte';
  import ToDoList from '$lib/components/ToDoList.svelte';
  import * as bootstrap from 'bootstrap';

  let novaTarefa = '';
  let tarefas = [];
  let tarefaEditando;
  let conteudoTarefaEditando = '';
  let tarefaExcluindo;
  let mensagemToast;
  let filtro = 'todas'; // opções: 'todas', 'pendentes', 'concluidas'

  function adicionarTarefa(event) {
    event.preventDefault();
    novaTarefa = novaTarefa.trim();
    if (!novaTarefa) {
      mensagemToast.show();
      return;
    }
    tarefas = [...tarefas, { conteudo: novaTarefa, status: 0 }];
    novaTarefa = '';
  }

  function marcarTodasComo(status) {
    tarefas = tarefas.map(t => ({ ...t, status }));
  }

  function definirFiltro(opcao) {
    filtro = opcao;
  }

  function editarTarefa(tarefa) {
    tarefaEditando = tarefa;
    conteudoTarefaEditando = tarefa.conteudo;
  }

  function confirmarEdicao() {
    conteudoTarefaEditando = conteudoTarefaEditando.trim();
    if (!conteudoTarefaEditando) {
      mensagemToast.show();
      return;
    }
    tarefaEditando.conteudo = conteudoTarefaEditando;
    cancelarEdicao();
  }

  function cancelarEdicao() {
    tarefaEditando = undefined;
    conteudoTarefaEditando = '';
  }

  function alterarStatus(tarefa, status) {
    tarefa.status = status;
    tarefas = [...tarefas];
  }

  function excluirTarefa(tarefa) {
    tarefaExcluindo = tarefa;
  }

  function confirmarExclusao() {
    tarefas = tarefas.filter(t => t !== tarefaExcluindo);
    tarefaExcluindo = undefined;
  }

  onMount(() => {
    mensagemToast = new bootstrap.Toast(document.querySelector('#mensagemToast'));
  });
</script>

<div class="fixed-top pt-5" style="z-index: 1020;">
  <form class="container-fluid input-group px-4 pt-3" on:submit={adicionarTarefa}>
    <input
      class="form-control form-control-lg"
      placeholder="Nova tarefa"
      bind:value={novaTarefa}
    />
    <button type="submit" class="btn btn-primary input-group-text" aria-label="adicionar">
      <i class="bi bi-plus-lg"></i>
    </button>
  </form>
  <Toast id="mensagemToast" msg="Digite algo!" />
</div>

<div class="container-fluid mt-5 pt-3">
  <div class="dropdown mb-3">
    <button class="btn btn-secondary dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false">
      Opções
    </button>
    <ul class="dropdown-menu">
      <li><a class="dropdown-item" on:click={() => marcarTodasComo(1)}>Marcar todas como concluídas</a></li>
      <li><a class="dropdown-item" on:click={() => marcarTodasComo(0)}>Marcar todas como pendentes</a></li>
      <li><a class="dropdown-item" on:click={() => definirFiltro('pendentes')}>Exibir pendentes no topo</a></li>
      <li><a class="dropdown-item" on:click={() => definirFiltro('concluidas')}>Exibir concluídas no topo</a></li>
      <li><a class="dropdown-item" on:click={() => definirFiltro('todas')}>Exibir todas no topo</a></li>
    </ul>
  </div>

  <ToDoList
    tarefas={
      filtro === 'todas'
        ? tarefas
        : [
            ...tarefas.filter(t => t.status === (filtro === 'pendentes' ? 0 : 1)),
            ...tarefas.filter(t => t.status !== (filtro === 'pendentes' ? 0 : 1))
          ]
    }
    {tarefaEditando}
    bind:conteudoTarefaEditando
    {editarTarefa}
    {confirmarEdicao}
    {cancelarEdicao}
    {alterarStatus}
    {excluirTarefa}
  />
</div>

<Modal msg="Deseja excluir a tarefa?" acao={confirmarExclusao} />
