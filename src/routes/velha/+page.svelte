<script>
	let tabuleiro = Array(9).fill("");
	let jogadorAtual = "X";
	let mensagem = "";

	function clicarCasa(i) {
		if (tabuleiro[i] === "") {
			tabuleiro[i] = jogadorAtual;
			jogadorAtual = jogadorAtual === "X" ? "O" : "X";
			mensagem = "";
			tabuleiro = [...tabuleiro];
		} else {
			mensagem = "Casa ocupada!";
		}
	}

	function reiniciar() {
		tabuleiro = Array(9).fill("");
		jogadorAtual = "X";
		mensagem = "";
		tabuleiro = [...tabuleiro];
	}
</script>

<style>
	.tabuleiro {
		display: grid;
		grid-template-columns: repeat(3, 80px);
		gap: 5px;
		justify-content: center;
		margin-top: 20px;
	}
	.casa {
		width: 80px;
		height: 80px;
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 2em;
		border: 1px solid #000;
		cursor: pointer;
		user-select: none;
	}
	.mensagem {
		color: red;
		text-align: center;
		margin-top: 10px;
	}
	button {
		display: block;
		margin: 20px auto 0;
		padding: 8px 16px;
	}
</style>

<div class="tabuleiro">
	{#each tabuleiro as casa, i}
		<div class="casa" on:click={() => clicarCasa(i)}>{casa}</div>
	{/each}
</div>

{#if mensagem}
	<div class="mensagem">{mensagem}</div>
{/if}

<button on:click={reiniciar}>Reiniciar</button>
