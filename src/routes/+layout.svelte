
  <script>
    import Header from '$lib/components/Header.svelte';
    import SideBar from '$lib/components/SideBar.svelte';
    import 'bootstrap/dist/css/bootstrap.css';
    import 'bootstrap-icons/font/bootstrap-icons.css';
    import * as bootstrap from 'bootstrap';
    import { onMount } from 'svelte';

    let hearts = [];

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

    let { children } = $props();
</script>

<div class="background-container">
    {#each hearts as heart (heart.id)}
        <div
            class="heart"
            style="left: {heart.left}%; animation-delay: {heart.delay}s; animation-duration: {heart.duration}s"
            aria-hidden="true"
        >❤️</div>
    {/each}
</div>

<Header />

<div class="container-fluid py-5 mt-1 mb-4">
    {@render children()}
</div>

<footer class="navbar bg-body-tertiary fixed-bottom">
    <div class="container-fluid">Made with SvelteKit, CapacitorJS, Drizzle, Sql.js and Bootstrap</div>
</footer>

<SideBar />

<style>
    .background-container {
        position: fixed;
        inset: 0;
        pointer-events: none;
        z-index: -1;
        background: linear-gradient(135deg, #ffe6f2, #d4c6eb);
    }

    .heart {
        position: absolute;
        bottom: 0;
        font-size: 1.6rem;
        color: #ff7ca2;
        opacity: 0.8;
        animation-name: heartFloat;
        animation-timing-function: ease-in;
        animation-iteration-count: infinite;
        user-select: none;
        user-drag: none;
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
        .heart {
            font-size: 1.2rem;
        }
    }
</style>