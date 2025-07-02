<script>
	let palavra = '';
	let resultado = '';

	function verificar() {
		const limpa = palavra.replace(/\s/g, '').toLowerCase();
		const invertida = limpa.split('').reverse().join('');
		resultado = limpa === invertida ? 'É palíndromo' : 'Não é palíndromo';
	}
</script>

<style>
	.container {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 100vh;
		background-color: #f9f9f9;
		font-family: Arial, sans-serif;
	}

	input {
		padding: 10px;
		font-size: 1rem;
		width: 250px;
		border: 1px solid #ccc;
		border-radius: 5px;
		margin-bottom: 10px;
	}

	button {
		padding: 10px 20px;
		font-size: 1rem;
		background-color: #007bff;
		color: white;
		border: none;
		border-radius: 5px;
		cursor: pointer;
		transition: background 0.3s;
	}

	button:hover {
		background-color: #0056b3;
	}

	p {
		margin-top: 20px;
		font-size: 1.2rem;
		color: #333;
	}
</style>

<div class="container">
	<input bind:value={palavra} placeholder="Digite uma palavra" />
	<button on:click={verificar}>Verificar</button>
	<p>{resultado}</p>
</div>
