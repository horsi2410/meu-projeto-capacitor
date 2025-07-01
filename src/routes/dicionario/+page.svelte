<script>
    // Importa o dicionário, que deve ser uma lista de objetos com as palavras e seus dados.
    import { dicionario } from '$lib/dicionario';

    let palavra = ''; // Variável que armazena o texto digitado pelo usuário.
    let filtradas = dicionario; // Lista de palavras filtradas, inicialmente igual ao dicionário completo.

    // Função que filtra as palavras com base no texto digitado.
    function buscar() {
        if (palavra === '') {
            // Caso o campo esteja vazio, exibe todas as palavras.
            filtradas = dicionario;
            return;
        }

        // Filtra as palavras do dicionário que incluem o termo digitado (ignorando maiúsculas/minúsculas).
        filtradas = dicionario.filter(termo =>
            termo.palavra.toLowerCase().includes(palavra.toLowerCase())
        );
    }
</script>

<!-- Campo de entrada para o usuário digitar uma palavra -->
<div>
    <input 
        placeholder="Digite uma palavra" 
        oninput={buscar} 
        bind:value={palavra} 
    />
</div>

<!-- Lista de palavras filtradas -->
<div>
    <ol>
        {#each filtradas as termo}
            <li>
                <!-- Cria um link para cada palavra filtrada -->
                <a href="/dicionario/{termo.palavra}">{termo.palavra}</a>
            </li>
        {/each}
    </ol>
</div>
