<script>
  let display = '';

  function clear() {
    display = '';
  }

  function backspace() {
    display = display.slice(0, -1);
  }

  function calc() {
    try {
      display = eval(display.replace('x', '*')).toString();
    } catch (error) {
      display = 'Error';
    }
  }

  function press(c) {
    display += c;
  }

  function changeSign() {
    try {
      display = (parseFloat(display) * -1).toString();
    } catch (error) {
      display = 'Error';
    }
  }

  function percentage() {
    try {
      display = (parseFloat(display) / 100).toString();
    } catch (error) {
      display = 'Error';
    }
  }

  function square() {
    try {
      display = (parseFloat(display) ** 2).toString();
    } catch (error) {
      display = 'Error';
    }
  }

  function squareRoot() {
    try {
      display = Math.sqrt(parseFloat(display)).toString();
    } catch (error) {
      display = 'Error';
    }
  }

  function round() {
    try {
      display = Math.round(parseFloat(display)).toString();
    } catch (error) {
      display = 'Error';
    }
  }

  function logBase10() {
    try {
      display = Math.log10(parseFloat(display)).toString();
    } catch (error) {
      display = 'Error';
    }
  }

  function modulo() {
    try {
      const divisor = prompt("Digite o divisor:");
      if (divisor !== null) {
        display = (parseFloat(display) % parseFloat(divisor)).toString();
      }
    } catch (error) {
      display = 'Error';
    }
  }
</script>

<style>

  .calculator {
    max-width: 400px;
    width: 100%;
    background: #f9fafb;
    border-radius: 0.75rem;
    box-shadow: 0 8px 24px rgb(149 157 165 / 0.2);
    padding: 1.5rem;
    user-select: none;
  }

  input.display {
    width: 100%;
    font-size: 2rem;
    font-weight: 700;
    padding: 0.75rem 1rem;
    border-radius: 0.5rem;
    border: 1px solid #d1d5db;
    margin-bottom: 1.5rem;
    text-align: right;
    color: #111827;
  }

  .buttons-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-auto-rows: 3.5rem;
    gap: 0.5rem;
  }

  button {
    font-weight: 600;
    border-radius: 0.5rem;
    border: none;
    cursor: pointer;
    user-select: none;
    font-size: 1.25rem;
    padding: 0.75rem 0;
    transition: background-color 0.3s, transform 0.2s;
    box-shadow: 0 1px 3px rgb(0 0 0 / 0.1);
  }



  button:focus-visible {
    outline: 2px solid #00df5d;
    outline-offset: 2px;
  }

  .btn-clear {
    background: #e40e0e;
    color: white;
  }

  .btn-clear:hover {
    background: #dc2626;
  }

  .btn-operator {
    background: #3b82f6;
    color: white;
  }

  .btn-operator:hover {
    background: #2563eb;
  }

  .btn-secondary {
    background: #6b7280;
    color: white;
  }

  .btn-secondary:hover {
    background: #4b5563;
  }

  .btn-number {
    background: #e5e7eb;
    color: #111827;
  }

  .btn-plus {
    grid-row: span 2;
    height: 100%;
  }
</style>

<div class="calculator">
  <input class="display" readonly value={display} />
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
