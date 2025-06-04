<script>
  let display = '';

  function clear() {
    display = '';
  }

  function calc() {
    display = eval(display.replace('x', '*')); // Replace 'x' with '*' for multiplication
  }

  function press(c) {
    display += c;
  }

  function changeSign() {
    if (display) {
      display = (eval(display) * -1).toString(); // Negate the current value
    }
  }

  function percentage() {
    if (display) {
      display = (eval(display) / 100).toString(); // Convert to percentage
    }
  }
</script>

<div class="text-center mt-3">
  <input class='form-control' readonly bind:value={display} />
  <table class='table table-sm table-borderless'>
    <tbody>
      <tr>
        <td><button type="button" class="btn btn-light w-100" onclick={() => clear()}>C</button></td>
        <td><button type="button" class="btn btn-outline-primary w-100" onclick={() => press('(')}>&lpar;</button></td>
        <td><button type="button" class="btn btn-outline-primary w-100" onclick={() => press(')')}>&rpar;</button></td>
        <td><button type="button" class="btn btn-info w-100" onclick={() => press('/')}>/</button></td>
      </tr>
      <tr>
        <td><button type="button" class="btn btn-warning w-100" onclick={() => press(7)}>7</button></td>
        <td><button type="button" class="btn btn-outline-primary w-100" onclick={() => press(8)}>8</button></td>
        <td><button type="button" class="btn btn-outline-primary w-100" onclick={() => press(9)}>9</button></td>
        <td><button type="button" class="btn btn-info w-100" onclick={() => press('x')}>x</button></td>
      </tr>
      <tr>
        <td><button type="button" class="btn btn-dark w-100" onclick={() => press(4)}>4</button></td>
        <td><button type="button" class="btn btn-outline-primary w-100" onclick={() => press(5)}>5</button></td>
        <td><button type="button" class="btn btn-outline-primary w-100" onclick={() => press(6)}>6</button></td>
        <td><button type="button" class="btn btn-info w-100" onclick={() => press('-')}>-</button></td>
      </tr>
      <tr>
        <td><button type="button" class="btn btn-danger w-100" onclick={() => press(1)}>1</button></td>
        <td><button type="button" class="btn btn-outline-primary w-100" onclick={() => press(2)}>2</button></td>
        <td><button type="button" class="btn btn-outline-primary w-100" onclick={() => press(3)}>3</button></td>
        <td rowspan='2' style='height: 0;'><button type="button" class="btn btn-info w-100 h-100" onclick={() => press('+')}>+</button></td>
      </tr>
      <tr>
        <td><button type="button" class="btn btn-outline-primary w-100" onclick={() => press(0)}>0</button></td>
        <td><button type="button" class="btn btn-info w-100" onclick={() => press('.')}>.</button></td>
        <td><button type="button" class="btn btn-info w-100" onclick={() => calc()}>=</button></td>
      </tr>
      <tr>
        <td>"<button type="button" class="btn btn-outline-danger w-100" onclick={() => changeSign()}>±</button></td>
        <td><button type="button" class="btn btn-secondary w-100" onclick={() => percentage()}>%</button></td>
      </tr>
    </tbody>
  </table>
</div>
