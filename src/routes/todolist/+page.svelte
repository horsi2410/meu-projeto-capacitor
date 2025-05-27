<script>
  import { onMount } from 'svelte';
  import Modal from '$lib/components/Modal.svelte';
  import Toast from '$lib/components/Toast.svelte';
  import ToDoList from '$lib/components/ToDoList.svelte';
  import * as bootstrap from 'bootstrap';

  let novaTarefa = $state('');
  let tarefas = $state([]);
  let tarefasPendentes = $derived(tarefas.filter(tarefa => tarefa.status == 0));
  let tarefasConcluidas = $derived(tarefas.filter(tarefa => tarefa.status == 1));
  let conteudoTarefaEditando = $state('');
  let tarefaEditando = $state();
  let tarefaExcluindo;
  let mostrarModal = $state(false);
  let mensagemToast;

  async function adicionarTarefa() {
    novaTarefa = novaTarefa.trim();
    if (!novaTarefa) {
      mensagemToast.show(); 
      return; 
    }
    
    tarefas.push({ id: Date.now(), conteudo: novaTarefa, status: 0 }); // Adiciona um id único para cada tarefa
    novaTarefa = ''; 
  }

  function editarTarefa(tarefa) {
    tarefaEditando = tarefa;
    conteudoTarefaEditando = tarefa.conteudo;
  }

  function confirmarEdicao() {
    if (!conteudoTarefaEditando.trim()) {
      mensagemToast.show();
      return;
    }
    
    tarefaEditando.conteudo = conteudoTarefaEditando.trim();
    tarefaEditando = undefined; 
  }

  function cancelarEdicao() {
    tarefaEditando = undefined; 
  }

  function excluirTarefa(tarefa) {
    tarefaExcluindo = tarefa;
    mostrarModal.set(true); // Exibe o modal para confirmação
  }

  function confirmarExclusao() {
    tarefas.set(tarefas.filter(tarefa => tarefa !== tarefaExcluindo)); // Atualiza o estado de tarefas
    tarefaExcluindo = undefined;
    mostrarModal.set(false); // Esconde o modal após a exclusão
  }

  function alterarStatus(tarefa, status) {
    tarefa.status = status;
  }

  onMount(() => {
    mensagemToast = new bootstrap.Toast('#mensagemToast');
  });
</script>

<div class="fixed-top pt-5" style="z-index: 1020;">
  <form class="container-fluid input-group px-4 pt-3" onsubmit={adicionarTarefa}>
    <input class="form-control form-control-lg" placeholder="Nova tarefa" bind:value={novaTarefa} />
    <button type="submit" class="btn btn-primary input-group-text" aria-label="adicionar"> 
      <i class="bi bi-plus-lg"></i> 
    </button>
  </form>
  <Toast msg={'Digite algo!'} />
</div>

<div class="container-fluid mt-5 pt-3">
  <ToDoList 
    tarefas={tarefasPendentes} 
    {tarefaEditando} 
    bind:conteudoTarefaEditando 
    {editarTarefa} 
    {confirmarEdicao} 
    {cancelarEdicao} 
    {alterarStatus} 
    {excluirTarefa} 
  />
  <hr />
  <ToDoList 
    tarefas={tarefasConcluidas} 
    {tarefaEditando} 
    bind:conteudoTarefaEditando 
    {editarTarefa} 
    {confirmarEdicao} 
    {cancelarEdicao} 
    {alterarStatus} 
    {excluirTarefa} 
  />
</div>

{#if mostrarModal}
  <Modal msg="Deseja excluir a tarefa?" acao={confirmarExclusao} />
{/if}

{#each tarefasPendentes as tarefa (tarefa.id)}
  <div class="dropdown">
    <button class="btn btn-secondary dropdown-toggle" type="button" id={`dropdownMenuButton-${tarefa.id}`} data-bs-toggle="dropdown" aria-expanded="false">
      Ações
    </button>
    <ul class="dropdown-menu" aria-labelledby={`dropdownMenuButton-${tarefa.id}`}>
      <li><button class="dropdown-item" onclick={() => editarTarefa(tarefa)}>Editar</button></li>
      <li><button class="dropdown-item" onclick={() => excluirTarefa(tarefa)}>Excluir</button></li>
      <li><button class="dropdown-item" onclick={() => alterarStatus(tarefa, 1)}>Marcar como Concluída</button></li>
      <li><button class="dropdown-item" onclick={() => alterarStatus(tarefa, 0)}>Marcar como Pendência</button></li>
    </ul>
  </div>
{/each}

{#each tarefasConcluidas as tarefa (tarefa.id)}
  <div class="dropdown">
    <button class="btn btn-secondary dropdown-toggle" type="button" id={`dropdownMenuButton-${tarefa.id}`} data-bs-toggle="dropdown" aria-expanded="false">
      Ações
    </button>
    <ul class="dropdown-menu" aria-labelledby={`dropdownMenuButton-${tarefa.id}`}>
      <li><button class="dropdown-item" onclick={() => editarTarefa(tarefa)}>Editar</button></li>
      <li><button class="dropdown-item" onclick={() => excluirTarefa(tarefa)}>Excluir</button></li>
      <li><button class="dropdown-item" onclick={() => alterarStatus(tarefa, 1)}>Marcar como Concluída</button></li>
      <li><button class="dropdown-item" onclick={() => alterarStatus(tarefa, 0)}>Marcar como Pendência</button></li>
    </ul>
  </div>
{/each}
