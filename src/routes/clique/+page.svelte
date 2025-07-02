<script>
	let count = 0;
	function increment() {
		count++;
	}
	function reset() {
		count = 0;
	}
</script>

<style>
	.container {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 100vh;
		background-color: #f0f4f8;
		font-family: Arial, sans-serif;
	}

	h1 {
		font-size: 2rem;
		color: #333;
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

	button:first-of-type {
		background-color: #4caf50;
		color: white;
	}

	button:first-of-type:hover {
		background-color: #45a049;
	}

	button:last-of-type {
		background-color: #f44336;
		color: white;
	}

	button:last-of-type:hover {
		background-color: #d32f2f;
	}
</style>

<div class="container">
	<h1>Contador: {count}</h1>
	<button on:click={increment}>Incrementar</button>
	<button on:click={reset}>Resetar</button>
</div>
