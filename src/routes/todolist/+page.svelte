<script>
  import { onMount } from 'svelte';
  import Modal from '$lib/components/Modal.svelte';
  import Toast from '$lib/components/Toast.svelte';
  import ToDoList from '$lib/components/ToDoList.svelte';
  import * as bootstrap from 'bootstrap';

  // Estado para nova tarefa no input
  let novaTarefa = '';
  // Lista das tarefas, cada uma é um objeto { conteudo: string, status: 0 ou 1 }
  let tarefas = [];
  // Tarefa que está sendo editada atualmente
  let tarefaEditando;
  // Conteúdo temporário para edição
  let conteudoTarefaEditando = '';
  // Tarefa que está marcada para exclusão
  let tarefaExcluindo;
  // Instância do Toast do Bootstrap para mensagens
  let mensagemToast;
  // Filtro atual para exibição: 'todas', 'pendentes', 'concluidas'
  let filtro = 'todas';

  // Adiciona nova tarefa após submeter o formulário
  function adicionarTarefa(event) {
    event.preventDefault();
    novaTarefa = novaTarefa.trim();
    if (!novaTarefa) {
      // Mostra mensagem caso o campo esteja vazio
      mensagemToast.show();
      return;
    }
    // Adiciona a nova tarefa à lista com status pendente (0)
    tarefas = [...tarefas, { conteudo: novaTarefa, status: 0 }];
    novaTarefa = '';
  }

  // Marca todas as tarefas como pendentes (0) ou concluídas (1)
  function marcarTodasComo(status) {
    tarefas = tarefas.map(t => ({ ...t, status }));
  }

  // Define o filtro para mostrar tarefas
  function definirFiltro(opcao) {
    filtro = opcao;
  }

  // Inicia edição da tarefa selecionada
  function editarTarefa(tarefa) {
    tarefaEditando = tarefa;
    conteudoTarefaEditando = tarefa.conteudo;
  }

  // Confirma a edição, salvando o conteúdo editado
  function confirmarEdicao() {
    conteudoTarefaEditando = conteudoTarefaEditando.trim();
    if (!conteudoTarefaEditando) {
      mensagemToast.show();
      return;
    }
    tarefaEditando.conteudo = conteudoTarefaEditando;
    cancelarEdicao();
  }

  // Cancela a edição da tarefa
  function cancelarEdicao() {
    tarefaEditando = undefined;
    conteudoTarefaEditando = '';
  }

  // Altera status (pendente/concluída) da tarefa
  function alterarStatus(tarefa, status) {
    tarefa.status = status;
    tarefas = [...tarefas]; // Atualiza lista para reatividade
  }

  // Marca tarefa para exclusão, abre modal de confirmação
  function excluirTarefa(tarefa) {
    tarefaExcluindo = tarefa;
  }

  // Confirma exclusão da tarefa selecionada
  function confirmarExclusao() {
    tarefas = tarefas.filter(t => t !== tarefaExcluindo);
    tarefaExcluindo = undefined;
  }

  // Inicializa o Toast após o componente montar
  onMount(() => {
    mensagemToast = new bootstrap.Toast(document.querySelector('#mensagemToast'));
  });
</script>

<!-- Área fixa no topo para adicionar nova tarefa e mostrar mensagens Toast -->
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
  <!-- Componente Toast para mostrar mensagens -->
  <Toast id="mensagemToast" msg="Digite algo!" />
</div>

<!-- Conteúdo principal com opções e lista de tarefas -->
<div class="container-fluid mt-5 pt-3">
  <!-- Dropdown para opções de marcação e filtro -->
  <div class="dropdown mb-3">
    <button
      class="btn btn-secondary dropdown-toggle"
      type="button"
      data-bs-toggle="dropdown"
      aria-expanded="false"
    >
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

  <!-- Componente que lista as tarefas filtradas e gerencia ações -->
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

<!-- Modal para confirmar exclusão da tarefa -->
<Modal msg="Deseja excluir a tarefa?" acao={confirmarExclusao} />
