<script>
  import { fly } from 'svelte/transition';
  import { cubicOut } from 'svelte/easing';
  import { onMount } from 'svelte';

  let elements = [];

  function createElements() {
      elements = Array.from({ length: 10 }).map(() => {
          return {
              id: Math.random().toString(36).substr(2, 9),
              left: Math.random() * 100,
              delay: Math.random() * 5,
              duration: 3 + Math.random() * 3
          };
      });
  }

  onMount(() => {
      createElements();
  });
</script>

<div class="container">
  <h1
      transition:fly="{{ y: 20, duration: 800, easing: cubicOut }}"
      class="elegant-heading"
  >
      Bem-vindo ao site do Guilherme
  </h1>

  {#each elements as element (element.id)}
      <div
          class="element"
          style="left: {element.left}%; animation-delay: {element.delay}s; animation-duration: {element.duration}s"
          aria-hidden="true"
      >❤️</div>
  {/each}
</div>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400&display=swap');


  .container {
      position: relative;
      min-height: 100vh;
      overflow: hidden; /* Garante que não haja rolagem */
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 1rem;
      background: url('/258979e3c40afda896103de6a1811be5.jpg') no-repeat center center fixed;
      background-size: cover;
      user-select: none;
  }

  .elegant-heading {
      font-family: 'Roboto', sans-serif;
      font-size: 2.4rem; /* Ajuste para dispositivos móveis */
      color: #ffffff; /* Cor branca para contraste */
      text-align: center;
      margin: 2rem 1rem; /* Reduzindo a margem */
      line-height: 1.3;
      position: relative;
      text-shadow: 0 0 10px rgba(0, 0, 0, 0.7); /* Sombra sutil para legibilidade */
  }

  .element {
      position: absolute;
      font-size: 1.4rem; /* Ajuste para dispositivos móveis */
      color: #ffcc00; /* Cor dourada para sofisticação */
      opacity: 0.8;
      animation-name: floatEffect;
      animation-timing-function: ease-in;
      animation-iteration-count: infinite;
  }

  @keyframes floatEffect {
      0% {
          transform: translateY(0) scale(1);
          opacity: 0.8;
      }
      100% {
          transform: translateY(-80px) scale(1.2); /* Reduzindo a altura do movimento */
          opacity: 0;
      }
  }

  @media (max-width: 600px) {
      .elegant-heading {
          font-size: 2rem; /* Tamanho menor para cabeçalho em dispositivos móveis */
      }
      .element {
          font-size: 1.2rem; /* Tamanho menor para elementos flutuantes */
      }
  }
</style>