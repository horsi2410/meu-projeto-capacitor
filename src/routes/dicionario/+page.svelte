<script>
    import {dicionario} from '$lib/dicionario';

    let palavra = $state('')
let filtradas = $state(dicionario)

    function buscar () {
        if ((palavra == '')) {
            filtradas == diconario;
        return;
        }
        filtradas= []
        for (const termo of dicionario) {
            if (termo.palavra.startsWith (palavra)) {
                filtradas.push (termo);
            }
        }

    }
</script>
<div>
<input  placeholder="Digite uma palavra" oniput={buscar} bind:value = {palavra}  />
</div>
<div>
    <ul>
        <ol>
    {#each filtradas as termo}
 <li><a href="/dicionario/ {termo.palavra}">{termo?.palavra}</a></li>
    {/each}
    </ol>
    </ul>

</div>