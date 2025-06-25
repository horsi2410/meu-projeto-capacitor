<script>
    let limite = 10; // Valor inicial do limite
    let resultado = '';
  
    function sortear() {
      if (limite > 0) {
        const numeroSorteado = Math.floor(Math.random() * limite) + 1;
        resultado = `Sorteado: ${numeroSorteado}`;
      } else {
        resultado = 'Defina um limite válido!';
      }
    }
  </script>
  
  <div style="padding: 20px; text-align: center; font-family: Arial, sans-serif;">
    <h1 style="font-size: 1.5em; margin-bottom: 10px;">Sorteio de Números</h1>
    
    <!-- Controle deslizante para definir o limite -->
    <label for="range" style="font-size: 1.2em;">Defina o limite: {limite}</label>
    <input
      id="range"
      type="range"
      min="1"
      max="20"
      bind:value={limite}
      style="width: 90%; margin: 10px 0;"
    />
    
    <button
      on:click={sortear}
      style="width: 90%; padding: 10px; background: #007BFF; color: white; font-size: 1em; border: none; border-radius: 8px; cursor: pointer;"
    >
      Sortear
    </button>
    
    {#if resultado}
      <p style="margin-top: 20px; font-size: 1.2em; font-weight: bold;">{resultado}</p>
    {/if}
  
    <br><br>
    <h1 class="nav-item">
      <a
        class="nav-link active"
        href="/sorteio"
        style="text-decoration: none; color: white; background: #4CAF50; padding: 10px; border-radius: 8px;"
      >
        Voltar
      </a>
    </h1>
  </div>
  