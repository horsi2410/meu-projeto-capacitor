<script>
    import { fly } from 'svelte/transition';
    import { cubicOut } from 'svelte/easing';
    import { onMount } from 'svelte';
  
    let hearts = [];
  
    // Generate random hearts positions and delays
    function createHearts() {
      hearts = Array.from({ length: 15 }).map(() => {
        return {
          id: Math.random().toString(36).substr(2, 9),
          left: Math.random() * 100,
          delay: Math.random() * 5,
          duration: 3 + Math.random() * 3
        };
      });
    }
  
    onMount(() => {
      createHearts();
    });
  </script>
  
  <div class="container">
    <h1
      transition:fly="{{ y: 20, duration: 800, easing: cubicOut }}"
      class="cute-heading"
    >
      Seja bem vindo ao site do Guilherme
      <span class="sparkle" aria-hidden="true"></span>
      <span class="sparkle" aria-hidden="true"></span>
      <span class="sparkle" aria-hidden="true"></span>
    </h1>
  
    <!-- Floating hearts -->
    {#each hearts as heart (heart.id)}
      <div
        class="heart"
        style="left: {heart.left}%; animation-delay: {heart.delay}s; animation-duration: {heart.duration}s"
        aria-hidden="true"
      >❤️</div>
    {/each}
  </div>
  
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Comic+Neue:wght@700&display=swap');
  
    /* Background gradient pastel pink/lavender */
    .container {
      position: relative;
      min-height: 100vh;
      overflow: hidden;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 1rem;
      background: linear-gradient(135deg, #ffe6f2, #d4c6eb);
      user-select: none;
    }
  
    .cute-heading {
      font-family: 'Comic Neue', cursive, sans-serif;
      font-size: 2.6rem;
      color: #ff6f91;
      text-align: center;
      margin: 3rem 1rem;
      line-height: 1.3;
      cursor: default;
      position: relative;
      user-select: none;
      text-shadow:
        0 0 6px #ff9bbf,
        0 0 12px #ff7ca2,
        0 0 18px #ff5d88;
      animation: bounceScale 3s cubic-bezier(0.645,0.045,0.355,1) infinite;
      z-index: 10;
    }
  
    /* Sparkle effect on heading */
    .sparkle {
      position: absolute;
      width: 10px;
      height: 10px;
      background: #fff;
      border-radius: 50%;
      filter: drop-shadow(0 0 3px #ff7ca2);
      animation: sparkleAnim 2.5s linear infinite;
      opacity: 0.9;
    }
    .sparkle:nth-child(2) {
      top: 10%;
      left: 20%;
      animation-delay: 0s;
    }
    .sparkle:nth-child(3) {
      top: 50%;
      right: 20%;
      animation-delay: 1.3s;
    }
    .sparkle:nth-child(4) {
      top: 30%;
      left: 50%;
      animation-delay: 2.1s;
    }
  
    @keyframes sparkleAnim {
      0%, 100% {
        opacity: 0.9;
        transform: scale(1);
      }
      50% {
        opacity: 0.3;
        transform: scale(1.3);
      }
    }
  
    /* Heading bounce and scale */
    @keyframes bounceScale {
      0%, 100% {
        transform: translateY(0) scale(1);
      }
      50% {
        transform: translateY(-12px) scale(1.05);
      }
    }
  
    /* Floating hearts */
    .heart {
      position: absolute;
      font-size: 1.6rem;
      color: #ff7ca2;
      opacity: 0.8;
      animation-name: heartFloat;
      animation-timing-function: ease-in;
      animation-iteration-count: infinite;
    }
    @keyframes heartFloat {
      0% {
        transform: translateY(0) scale(1);
        opacity: 0.8;
      }
      100% {
        transform: translateY(-120px) scale(1.3);
        opacity: 0;
      }
    }
  
    /* Responsive scaling */
    @media (max-width: 600px) {
      .cute-heading {
        font-size: 2.2rem;
        margin: 2rem 1rem;
      }
      .heart {
        font-size: 1.2rem;
      }
    }
  
  </style>