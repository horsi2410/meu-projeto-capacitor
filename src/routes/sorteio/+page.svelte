<script>
    import { cubicOut } from 'svelte/easing';
    import { tweened } from 'svelte/motion';
    import { onMount } from 'svelte';
  
    export let titulo = "Sorteio na Roleta";
  
    let elementos = [];
    let novoElemento = '';
    let resultado = '';
    let girando = false;
    let angulo = tweened(0, { duration: 4000, easing: cubicOut });
  
    let confeteCanvas;
    let ctx;
    let confetes = [];
    let confeteAnimation;
  
    // Paleta de cores uniforme
    const cores = [
      "#F44336", "#E91E63", "#9C27B0", "#673AB7",
      "#3F51B5", "#2196F3", "#03A9F4", "#00BCD4",
      "#009688", "#4CAF50", "#8BC34A", "#CDDC39",
      "#FFEB3B", "#FFC107", "#FF9800", "#FF5722"
    ];
  
    function adicionarElemento() {
      if (novoElemento.trim() !== '') {
        elementos = [...elementos, novoElemento];
        novoElemento = '';
        resultado = '';
      }
    }
  
    function iniciarSorteio() {
      if (elementos.length === 0 || girando) return;
  
      girando = true;
      resultado = '';
  
      const itemIndex = Math.floor(Math.random() * elementos.length);
      const fatia = 360 / elementos.length;
      const voltaExtra = 360 * 3;
  
      // Ajusta para seta no topo (0º), alinhando o centro da fatia sorteada com a seta
      const alvo = voltaExtra + (360 - (itemIndex * fatia) - fatia / 2);
  
      angulo.set(alvo);
  
      setTimeout(() => {
        resultado = elementos[itemIndex];
        girando = false;
        startConfete();
      }, 4000);
    }
  
    function createConfete() {
      return {
        x: Math.random() * confeteCanvas.width,
        y: Math.random() * confeteCanvas.height - confeteCanvas.height,
        r: Math.random() * 6 + 4,
        d: Math.random() * 10 + 10,
        color: `hsl(${Math.random() * 360}, 100%, 50%)`,
        tilt: Math.floor(Math.random() * 10) - 10,
        tiltAngle: 0,
        tiltAngleIncrement: Math.random() * 0.07 + 0.05,
      };
    }
  
    function drawConfetes() {
      ctx.clearRect(0, 0, confeteCanvas.width, confeteCanvas.height);
      confetes.forEach(c => {
        ctx.beginPath();
        ctx.lineWidth = c.r / 2;
        ctx.strokeStyle = c.color;
        ctx.moveTo(c.x + c.tilt + c.r / 4, c.y);
        ctx.lineTo(c.x + c.tilt, c.y + c.tilt + c.r / 4);
        ctx.stroke();
      });
      updateConfetes();
    }
  
    function updateConfetes() {
      confetes.forEach(c => {
        c.tiltAngle += c.tiltAngleIncrement;
        c.y += (Math.cos(c.d) + 3 + c.r / 2) / 2;
        c.x += Math.sin(c.d);
        c.tilt = Math.sin(c.tiltAngle) * 15;
  
        if (c.y > confeteCanvas.height) {
          c.x = Math.random() * confeteCanvas.width;
          c.y = -10;
          c.tilt = Math.floor(Math.random() * 10) - 10;
        }
      });
    }
  
    function confeteLoop() {
      drawConfetes();
      confeteAnimation = requestAnimationFrame(confeteLoop);
    }
  
    function startConfete() {
      confetes = [];
      for (let i = 0; i < 250; i++) {
        confetes.push(createConfete());
      }
      confeteLoop();
      setTimeout(stopConfete, 7000);
    }
  
    function stopConfete() {
      cancelAnimationFrame(confeteAnimation);
      ctx.clearRect(0, 0, confeteCanvas.width, confeteCanvas.height);
    }
  
    onMount(() => {
      ctx = confeteCanvas.getContext('2d');
      confeteCanvas.width = 423;
      confeteCanvas.height = 867;
    });
  </script>
  
  <style>
    .container {
      max-width: 423px;
      margin: 0 auto;
      padding: 1rem;
      height: 867px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: space-between;
      position: relative;
      background: linear-gradient(135deg, #ff9a9e 0%, #fad0c4 99%, #fad0c4 100%);
    }
  
    h1 {
      color: #3b0a45;
      font-weight: 900;
      font-size: 2.5rem;
      text-shadow: 2px 2px 8px rgba(0, 0, 0, 0.15);
    }
  
    .roleta-wrapper {
      position: relative;
      width: 320px;
      height: 320px;
      margin-top: 40px; /* espaço pra seta */
    }
  
    .roleta {
      position: relative;
      width: 320px;
      height: 320px;
      border-radius: 50%;
      border: 8px solid #fff;
      box-shadow: 0 0 30px rgba(59, 10, 69, 0.6);
      overflow: hidden;
      cursor: pointer;
      transition: box-shadow 0.3s ease;
      margin: 0 auto;
      background: white;
    }
  
    .roleta:hover {
      box-shadow: 0 0 50px #ff4081;
    }
  
    /* Seta em cima, apontando pra baixo */
    .seta-roleta {
      position: absolute;
      top: 0;
      left: 50%;
      transform: translate(-50%, -100%);
      width: 50px;
      height: 75px;
      filter: drop-shadow(0 0 5px #ff4081);
      cursor: default;
      user-select: none;
    }
  
    ul.lista-itens {
      max-height: 150px;
      overflow-y: auto;
      width: 100%;
      padding: 0;
      margin: 0;
      list-style: none;
    }
  
    ul.lista-itens li {
      background: rgba(255, 255, 255, 0.85);
      margin: 5px 0;
      padding: 8px 12px;
      border-radius: 6px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-weight: 600;
      color: #3b0a45;
      box-shadow: 0 2px 5px rgba(59, 10, 69, 0.1);
    }
  
    ul.lista-itens li button {
      background: transparent;
      border: none;
      color: #e91e63;
      font-weight: bold;
      cursor: pointer;
      font-size: 1.1rem;
      line-height: 1;
    }
  
    .input-group {
      display: flex;
      width: 100%;
      max-width: 423px;
      margin: 0 auto 1rem;
    }
  
    input[type="text"] {
      flex: 1;
      padding: 12px 16px;
      border: none;
      border-radius: 30px 0 0 30px;
      font-size: 1rem;
      outline: none;
      box-shadow: 0 2px 10px rgba(59, 10, 69, 0.1);
    }
  
    button.adicionar-btn {
      padding: 12px 24px;
      background: #ff4081;
      border: none;
      color: white;
      font-weight: 700;
      border-radius: 0 30px 30px 0;
      cursor: pointer;
      box-shadow: 0 2px 10px #ff80ab;
      transition: background-color 0.3s ease;
    }
    button.adicionar-btn:hover {
      background: #c2185b;
    }
  
    button.sortear-btn {
      background: #3b0a45;
      color: white;
      padding: 18px 50px;
      font-size: 1.4rem;
      font-weight: 800;
      border-radius: 50px;
      box-shadow: 0 0 30px #f48fb1;
      cursor: pointer;
      margin-bottom: 1rem;
      transition: all 0.3s ease;
    }
    button.sortear-btn:hover {
      background: #6a1b9a;
      transform: scale(1.05);
    }
  
    .resultado {
      font-size: 1.8rem;
      font-weight: 900;
      color: #6a1b9a;
      text-align: center;
      min-height: 2.5rem;
      padding: 10px 20px;
      border-radius: 15px;
      background: rgba(255, 255, 255, 0.7);
      box-shadow: 0 0 20px #ce93d8;
      user-select: none;
    }
  
    canvas {
      position: fixed;
      pointer-events: none;
      top: 0;
      left: 0;
      width: 423px;
      height: 867px;
      z-index: 9999;
    }
  
    /* Overlay do resultado centralizado */
    .overlay {
      position: fixed;
      top: 0; left: 0;
      width: 100vw; height: 100vh;
      background: rgba(0,0,0,0.5);
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 10000;
    }
  
    .resultado-central {
      background: white;
      padding: 2rem 3rem;
      border-radius: 15px;
      font-size: 2.5rem;
      font-weight: 900;
      color: #6a1b9a;
      box-shadow: 0 0 30px #ce93d8;
      position: relative;
      text-align: center;
      max-width: 90vw;
      user-select: none;
    }
  
    .fechar-btn {
      position: absolute;
      top: 12px;
      right: 12px;
      background: transparent;
      border: none;
      font-size: 2.5rem;
      font-weight: 900;
      color: #6a1b9a;
      cursor: pointer;
      line-height: 1;
      padding: 0;
      user-select: none;
      transition: color 0.3s ease;
    }
  
    .fechar-btn:hover {
      color: #ff4081;
    }
  </style>
  
  <div class="container">
    <h1>{titulo}</h1>
  
    <div class="roleta-wrapper">
      <!-- Seta em cima apontando pra baixo -->
      <svg
        class="seta-roleta"
        viewBox="0 0 64 96"
        xmlns="http://www.w3.org/2000/svg"
        aria-hidden="true"
      >
        <defs>
          <filter id="shadow" x="-20%" y="-20%" width="140%" height="140%">
            <feDropShadow
              dx="0"
              dy="0"
              stdDeviation="3"
              flood-color="#ff4081"
              flood-opacity="0.7"
            />
          </filter>
        </defs>
        <path
          fill="#ff4081"
          filter="url(#shadow)"
          d="M32 96 L0 32 L16 32 L16 0 L48 0 L48 32 L64 32 Z"
        />
      </svg>
  
      <div
        class="roleta"
        style="transform: rotate({$angulo}deg)"
        on:click={iniciarSorteio}
        aria-label="Roleta do sorteio"
      >
        {#if elementos.length > 0}
          <svg
            viewBox="0 0 320 320"
            width="320"
            height="320"
            style="position:absolute; top:0; left:0; pointer-events:none;"
          >
            {#each elementos as elemento, i}
              {#key elemento}
                {@const fatia = 360 / elementos.length}
                {@const anguloMeio = i * fatia + fatia / 2}
                {@const rad = (anguloMeio - 90) * Math.PI / 180}
                {@const raioTexto = 110}
                {@const xTexto = 160 + raioTexto * Math.cos(rad)}
                {@const yTexto = 160 + raioTexto * Math.sin(rad)}
                {@const rotacaoTexto = (anguloMeio > 90 && anguloMeio < 270) ? anguloMeio + 180 : anguloMeio}
                <g>
                  <path
                    d={`M160 160 L160 20 A140 140 0 0 1 ${160 + 140 * Math.cos((i + 1) * fatia * Math.PI / 180 - Math.PI / 2)} ${160 + 140 * Math.sin((i + 1) * fatia * Math.PI / 180 - Math.PI / 2)} Z`}
                    fill={cores[i % cores.length]}
                    stroke="#6a1b9a"
                    stroke-width="2"
                    transform={`rotate(${i * fatia} 160 160)`}
                  />
                  <text
                    x={xTexto}
                    y={yTexto}
                    text-anchor="middle"
                    alignment-baseline="middle"
                    fill="#3b0a45"
                    font-weight="700"
                    font-size="14"
                    transform={`rotate(${rotacaoTexto} ${xTexto} ${yTexto})`}
                    style="user-select:none;"
                  >
                    {elemento}
                  </text>
                </g>
              {/key}
            {/each}
          </svg>
        {:else}
          <p style="color:#6a1b9a; font-weight:700; text-align:center; margin-top:140px;">
            Adicione itens para montar a roleta
          </p>
        {/if}
      </div>
    </div>
  
    <ul class="lista-itens">
      {#each elementos as elemento, index}
        <li>
          {elemento}
          <button
            aria-label={"Remover " + elemento}
            on:click={() => (elementos = elementos.filter((_, i) => i !== index))}
            >×</button
          >
        </li>
      {/each}
    </ul>
  
    <div class="input-group">
      <input
        type="text"
        placeholder="Digite um item"
        bind:value={novoElemento}
        on:keydown={(e) => e.key === "Enter" && adicionarElemento()}
        aria-label="Novo item para a roleta"
        disabled={girando}
      />
      <button class="adicionar-btn" on:click={adicionarElemento} disabled={girando}>
        Adicionar
      </button>
    </div>
  
    <button
      class="sortear-btn"
      on:click={iniciarSorteio}
      disabled={girando || elementos.length === 0}
      aria-live="polite"
    >
      {girando ? "Girando..." : "Sortear"}
    </button>
  
    <div class="resultado" aria-live="polite">{resultado}</div>
  
    {#if resultado && !girando}
      <div class="overlay" role="dialog" aria-modal="true" aria-labelledby="resultadoTitulo" tabindex="-1">
        <div class="resultado-central">
          <h2 id="resultadoTitulo" style="margin-bottom:1rem;">Resultado:</h2>
          <p>{resultado}</p>
          <button class="fechar-btn" on:click={() => resultado = ''} aria-label="Fechar resultado">×</button>
        </div>
      </div>
    {/if}
  
    <canvas bind:this={confeteCanvas}></canvas>
  </div>
  