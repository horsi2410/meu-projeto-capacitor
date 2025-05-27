<script>
    const tarefasAFazer = $state(['Amor', 'arrumar um amor', 'comprar chocolate pro meu amor', 'fazer compras pro meu amor', 'estudar para prova para dar um futuro pro meu amor']);
    const tarefasConcluidas = $state([]);
    let tarefaNova = $state('');
    let tarefaEditandoIndice = $state(null);
    let tarefaEditando = $state('');
 
    function adicionarTarefa() {
        if (tarefaNova.trim()) {
            tarefasAFazer.push(tarefaNova);
            tarefaNova = '';
        }
    }
 
    function excluirTarefa(i, lista) {
        lista.splice(i, 1);
    }
 
    function editarTarefa(i, lista) {
        tarefaEditandoIndice = i;
        tarefaEditando = lista[i];
    }
 
    function salvarTarefa(i, lista) {
        lista[i] = tarefaEditando.trim();
        tarefaEditandoIndice = null;
    }
 
    function cancelarEdicao() {}
    
    function marcarComoConcluida(i) {
        const tarefaConcluida = tarefasAFazer[i];
        tarefasAFazer.splice(i, 1);
        tarefasConcluidas.push(tarefaConcluida);
    }

    function desmarcarComoConcluida(i) {
        const tarefa = tarefasConcluidas[i];
        tarefasConcluidas.splice(i, 1);
        tarefasAFazer.push(tarefa);
    }
</script>
 
<h2>Lista de tarefas a fazer</h2>
<p>
    <input placeholder="Digite a tarefa..." bind:value={tarefaNova} />
    <button onclick={adicionarTarefa}>➕</button>
</p>
<ul>
    {#each tarefasAFazer as tarefa, i}
        <li>
            {#if tarefaEditandoIndice == i}
                <input bind:value={tarefaEditando} />
                <button onclick={() => salvarTarefa(i, tarefasAFazer)}>💾</button>
                <button onclick={cancelarEdicao}>❌</button>
            {:else}
                {tarefa}
                <button onclick={() => editarTarefa(i, tarefasAFazer)}>✏</button>
                <button onclick={() => excluirTarefa(i, tarefasAFazer)}>🗑</button>
                <button onclick={() => marcarComoConcluida(i)}>✔️</button>
            {/if}
        </li>
    {/each}
</ul>

<h2>Tarefas concluídas</h2>
<ul>
    {#each tarefasConcluidas as tarefa, i}
        <li>
            {#if tarefaEditandoIndice == i}
                <input bind:value={tarefaEditando} />
                <button onclick={() => salvarTarefa(i, tarefasConcluidas)}>💾</button>
                <button onclick={cancelarEdicao}>❌</button>
            {:else}
                {tarefa}
                <button onclick={() => editarTarefa(i, tarefasConcluidas)}>✏</button>
                <button onclick={() => excluirTarefa(i, tarefasConcluidas)}>🗑</button>
                <button onclick={() => desmarcarComoConcluida(i)}>↩️</button>
            {/if}
        </li>
    {/each}
</ul>