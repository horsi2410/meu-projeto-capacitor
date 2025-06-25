<script>
    import { dicionario } from '$lib/dicionario';

    let palavra = ''; 
    let filtradas = dicionario; 

    function buscar() {
        if (palavra === '') {
            filtradas = dicionario; 
            return;
        }

        filtradas = dicionario.filter(termo =>
  termo.palavra.toLowerCase().includes(palavra.toLowerCase())
);

    }
</script>

<div>
    <input 
        placeholder="Digite uma palavra" 
        oninput={buscar} 
        bind:value={palavra} 
    />
</div>

<div>
    <ol>
        {#each filtradas as termo}
            <li>
                <a href="/dicionario/{termo.palavra}">{termo.palavra}</a>
            </li>
        {/each}
    </ol>
</div>
