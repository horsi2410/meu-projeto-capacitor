<script>
	import { onDestroy } from 'svelte';

	let time = 0;
	let interval = null;

	function start() {
		if (interval === null) {
			interval = setInterval(() => {
				time += 1;
			}, 1000);
		}
	}

	function stop() {
		clearInterval(interval);
		interval = null;
	}

	function reset() {
		stop();
		time = 0;
	}

	onDestroy(() => {
		clearInterval(interval);
	});
</script>

<style>
	.container {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 100vh;
		background-color: #e0f7fa;
		font-family: Arial, sans-serif;
	}

	h1 {
		font-size: 2rem;
		color: #00796b;
		margin-bottom: 20px;
	}

	button {
		margin: 5px;
		padding: 10px 20px;
		font-size: 1rem;
		border: none;
		border-radius: 5px;
		cursor: pointer;
		transition: background 0.3s;
	}

	button:nth-of-type(1) {
		background-color: #388e3c;
		color: white;
	}

	button:nth-of-type(1):hover {
		background-color: #2e7d32;
	}

	button:nth-of-type(2) {
		background-color: #fbc02d;
		color: white;
	}

	button:nth-of-type(2):hover {
		background-color: #f9a825;
	}

	button:nth-of-type(3) {
		background-color: #d32f2f;
		color: white;
	}

	button:nth-of-type(3):hover {
		background-color: #c62828;
	}
</style>

<div class="container">
	<h1>Tempo: {time}s</h1>
	<button on:click={start}>Iniciar</button>
	<button on:click={stop}>Parar</button>
	<button on:click={reset}>Resetar</button>
</div>
