<script>
  // Variável que armazena o texto exibido na tela da calculadora.
  let display = '';

  // Função para limpar completamente o display.
  function clear() {
    display = '';
  }

  // Função para apagar o último caractere do display.
  function backspace() {
    display = display.slice(0, -1);
  }

  // Função para calcular a expressão matemática no display.
  function calc() {
    try {
      // Avalia a expressão. Substitui 'x' por '*' para multiplicação.
      display = eval(display.replace('x', '*')).toString();
    } catch (error) {
      display = 'Error'; // Exibe "Error" caso a expressão seja inválida.
    }
  }

  // Função para adicionar caracteres pressionados ao display.
  function press(c) {
    display += c;
  }

  // Função para alterar o sinal do número no display.
  function changeSign() {
    try {
      display = (parseFloat(display) * -1).toString();
    } catch (error) {
      display = 'Error';
    }
  }

  // Função para calcular o valor percentual.
  function percentage() {
    try {
      display = (parseFloat(display) / 100).toString();
    } catch (error) {
      display = 'Error';
    }
  }

  // Função para calcular o quadrado do número no display.
  function square() {
    try {
      display = (parseFloat(display) ** 2).toString();
    } catch (error) {
      display = 'Error';
    }
  }

  // Função para calcular a raiz quadrada.
  function squareRoot() {
    try {
      display = Math.sqrt(parseFloat(display)).toString();
    } catch (error) {
      display = 'Error';
    }
  }

  // Função para arredondar o número.
  function round() {
    try {
      display = Math.round(parseFloat(display)).toString();
    } catch (error) {
      display = 'Error';
    }
  }

  // Função para calcular o logaritmo de base 10.
  function logBase10() {
    try {
      display = Math.log10(parseFloat(display)).toString();
    } catch (error) {
      display = 'Error';
    }
  }

  // Função para calcular o módulo (resto da divisão).
  function modulo() {
    try {
      const divisor = prompt("Digite o divisor:"); // Solicita o divisor ao usuário.
      if (divisor !== null) {
        display = (parseFloat(display) % parseFloat(divisor)).toString();
      }
    } catch (error) {
      display = 'Error';
    }
  }
</script>

<style>
  /* Estiliza o contêiner principal da calculadora */
  .calculator {
    max-width: 400px;
    width: 100%;
    background: #f9fafb; /* Fundo claro */
    border-radius: 0.75rem; /* Bordas arredondadas */
    box-shadow: 0 8px 24px rgb(149 157 165 / 0.2); /* Sombra suave */
    padding: 1.5rem;
    user-select: none; /* Desativa a seleção de texto */
  }

  /* Estiliza o display da calculadora */
  input.display {
    width: 100%;
    font-size: 2rem; /* Texto grande para visibilidade */
    font-weight: 700;
    padding: 0.75rem 1rem;
    border-radius: 0.5rem; /* Bordas arredondadas */
    border: 1px solid #d1d5db;
    margin-bottom: 1.5rem;
    text-align: right; /* Texto alinhado à direita */
    color: #111827; /* Texto escuro */
  }

  /* Configura o grid para os botões */
  .buttons-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr); /* Quatro colunas iguais */
    grid-auto-rows: 3.5rem;
    gap: 0.5rem; /* Espaçamento entre botões */
  }

  /* Estilização básica dos botões */
  button {
    font-weight: 600;
    border-radius: 0.5rem; /* Bordas arredondadas */
    border: none;
    cursor: pointer; /* Muda o cursor para "mãozinha" */
    user-select: none;
    font-size: 1.25rem; /* Tamanho do texto */
    padding: 0.75rem 0;
    transition: background-color 0.3s, transform 0.2s; /* Animações suaves */
    box-shadow: 0 1px 3px rgb(0 0 0 / 0.1); /* Sombra leve */
  }

  /* Botão de limpar (C) */
  .btn-clear {
    background: #e40e0e; /* Vermelho */
    color: white; /* Texto branco */
  }
  .btn-clear:hover {
    background: #dc2626; /* Vermelho mais escuro ao passar o mouse */
  }

  /* Botões de operadores */
  .btn-operator {
    background: #3b82f6; /* Azul */
    color: white;
  }
  .btn-operator:hover {
    background: #2563eb; /* Azul mais escuro ao passar o mouse */
  }

  /* Botões de funções secundárias */
  .btn-secondary {
    background: #6b7280; /* Cinza */
    color: white;
  }
  .btn-secondary:hover {
    background: #4b5563; /* Cinza mais escuro ao passar o mouse */
  }

  /* Botões numéricos */
  .btn-number {
    background: #e5e7eb; /* Fundo claro */
    color: #111827; /* Texto escuro */
  }

  /* Botão de soma ocupa duas linhas */
  .btn-plus {
    grid-row: span 2;
    height: 100%;
  }
</style>

<div class="calculator">
  <!-- Display da calculadora -->
  <input class="display" readonly value={display} />

  <!-- Grid de botões -->
  <div class="buttons-grid">
    <button class="btn-clear" onclick={clear}>C</button>
    <button class="btn-secondary" onclick={backspace}>⌫</button>
    <button class="btn-secondary" onclick={() => press('(')}>(</button>
    <button class="btn-secondary" onclick={() => press(')')}>)</button>
    <button class="btn-operator" onclick={() => press('/')}>/</button>
    <button class="btn-number" onclick={() => press('7')}>7</button>
    <button class="btn-number" onclick={() => press('8')}>8</button>
    <button class="btn-number" onclick={() => press('9')}>9</button>
    <button class="btn-operator" onclick={() => press('x')}>×</button>
    <button class="btn-number" onclick={() => press('4')}>4</button>
    <button class="btn-number" onclick={() => press('5')}>5</button>
    <button class="btn-number" onclick={() => press('6')}>6</button>
    <button class="btn-operator" onclick={() => press('-')}>-</button>
    <button class="btn-number" onclick={() => press('1')}>1</button>
    <button class="btn-number" onclick={() => press('2')}>2</button>
    <button class="btn-number" onclick={() => press('3')}>3</button>
    <button class="btn-operator btn-plus" onclick={() => press('+')}>+</button>
    <button class="btn-number" onclick={() => press('0')}>0</button>
    <button class="btn-number" onclick={() => press('.')}>.</button>
    <button class="btn-operator" onclick={calc}>=</button>
    <button class="btn-secondary" onclick={changeSign}>±</button>
    <button class="btn-secondary" onclick={percentage}>%</button>
    <button class="btn-secondary" onclick={square}>x²</button>
    <button class="btn-secondary" onclick={squareRoot}>√x</button>
    <button class="btn-secondary" onclick={round}>arred</button>
    <button class="btn-secondary" onclick={logBase10}>log10</button>
    <button class="btn-secondary" onclick={modulo}>resto</button>
  </div>
</div>
