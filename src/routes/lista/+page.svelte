<script>
    // Lista de tarefas pendentes, inicializada com exemplos de tarefas.
    const tarefasAFazer = $state([
        'Amor',
        'arrumar um amor',
        'comprar chocolate pro meu amor',
        'fazer compras pro meu amor',
        'estudar para prova para dar um futuro pro meu amor',
    ]);

    // Lista de tarefas concluídas.
    const tarefasConcluidas = $state([]);

    // Armazena o valor da nova tarefa que será adicionada.
    let tarefaNova = $state('');

    // Indica o índice da tarefa que está sendo editada, ou `null` se nenhuma tarefa estiver sendo editada.
    let tarefaEditandoIndice = $state(null);

    // Armazena o valor da tarefa enquanto ela está sendo editada.
    let tarefaEditando = $state('');

    // Função para adicionar uma nova tarefa à lista de tarefas pendentes.
    function adicionarTarefa() {
        if (tarefaNova.trim()) {
            tarefasAFazer.push(tarefaNova); // Adiciona a nova tarefa.
            tarefaNova = ''; // Limpa o campo de entrada.
        }
    }

    // Função para excluir uma tarefa de uma lista (pendentes ou concluídas).
    function excluirTarefa(i, lista) {
        lista.splice(i, 1); // Remove a tarefa pelo índice.
    }

    // Função para iniciar a edição de uma tarefa.
    function editarTarefa(i, lista) {
        tarefaEditandoIndice = i; // Define o índice da tarefa em edição.
        tarefaEditando = lista[i]; // Copia o valor da tarefa para edição.
    }

    // Função para salvar as alterações feitas em uma tarefa.
    function salvarTarefa(i, lista) {
        lista[i] = tarefaEditando.trim(); // Salva o valor editado na lista.
        tarefaEditandoIndice = null; // Cancela o modo de edição.
    }

    // Função para cancelar a edição de uma tarefa.
    function cancelarEdicao() {
        tarefaEditandoIndice = null; // Sai do modo de edição.
    }

    // Função para mover uma tarefa de "pendentes" para "concluídas".
    function marcarComoConcluida(i) {
        const tarefaConcluida = tarefasAFazer[i]; // Captura a tarefa concluída.
        tarefasAFazer.splice(i, 1); // Remove da lista de pendentes.
        tarefasConcluidas.push(tarefaConcluida); // Adiciona à lista de concluídas.
    }

    // Função para mover uma tarefa de "concluídas" para "pendentes".
    function desmarcarComoConcluida(i) {
        const tarefa = tarefasConcluidas[i]; // Captura a tarefa.
        tarefasConcluidas.splice(i, 1); // Remove da lista de concluídas.
        tarefasAFazer.push(tarefa); // Adiciona à lista de pendentes.
    }
</script>

<h2>Lista de tarefas a fazer</h2>
<p>
    <!-- Campo de entrada para adicionar novas tarefas -->
    <input placeholder="Digite a tarefa..." bind:value={tarefaNova} />
    <button onclick={adicionarTarefa}>➕</button>
</p>

<!-- Lista de tarefas pendentes -->
<ul>
    {#each tarefasAFazer as tarefa, i}
        <li>
            {#if tarefaEditandoIndice == i}
                <!-- Modo de edição -->
                <input bind:value={tarefaEditando} />
                <button onclick={() => salvarTarefa(i, tarefasAFazer)}>💾</button>
                <button onclick={cancelarEdicao}>❌</button>
            {:else}
                <!-- Exibição padrão -->
                {tarefa}
                <button onclick={() => editarTarefa(i, tarefasAFazer)}>✏</button>
                <button onclick={() => excluirTarefa(i, tarefasAFazer)}>🗑</button>
                <button onclick={() => marcarComoConcluida(i)}>✔️</button>
            {/if}
        </li>
    {/each}
</ul>

<h2>Tarefas concluídas</h2>
<!-- Lista de tarefas concluídas -->
<ul>
    {#each tarefasConcluidas as tarefa, i}
        <li>
            {#if tarefaEditandoIndice == i}
                <!-- Modo de edição -->
                <input bind:value={tarefaEditando} />
                <button onclick={() => salvarTarefa(i, tarefasConcluidas)}>💾</button>
                <button onclick={cancelarEdicao}>❌</button>
            {:else}
                <!-- Exibição padrão -->
                {tarefa}
                <button onclick={() => editarTarefa(i, tarefasConcluidas)}>✏</button>
                <button onclick={() => excluirTarefa(i, tarefasConcluidas)}>🗑</button>
                <button onclick={() => desmarcarComoConcluida(i)}>↩️</button>
            {/if}
        </li>
    {/each}
</ul>
