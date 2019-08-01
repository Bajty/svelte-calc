<script>
  import { afterUpdate } from "svelte";

  let value = 0;
  let displayValue = "0";
  let actualFunction = null;
  let result = null;
  let lastValue = 0;
  let resetDisplay = false;

  afterUpdate(() => {
    value = parseFloat(displayValue);
  });

  const clearDisplay = () => {
    displayValue = "0";
    value = 0;
    resetDisplay = false;
  };

  const computeResult = () => {
    if (result == null) {
      result = lastValue;
      return;
    }
    switch (actualFunction) {
      case "+":
        result = result + lastValue;
        break;
      case "-":
        result = result - lastValue;
        break;
      case "*":
        result = result * lastValue;
        break;
      case "/":
        result = result / lastValue;
        break;
      case "%":
        result = result / 100;
        break;
      default:
        result = null;
        actualFunction = null;
    }
    result = parseFloat(result.toFixed(10));
  };

  const operationClick = e => {
    if (!resetDisplay) {
      lastValue = value;
      computeResult();
      displayValue = result.toString();
    }
    actualFunction = e;
    resetDisplay = true;
  };

  const functionClick = e => {
    switch (e) {
      case "AC":
        displayValue = "0";
        actualFunction = null;
        result = null;
        lastValue = 0;
        resetDisplay = false;
        break;
      case "=":
        if (!resetDisplay) lastValue = value;
        if (actualFunction != null) computeResult();
        else {
          actualFunction = "+";
          result = value;
        }
        displayValue = result.toString();
        resetDisplay = true;
        break;
      case "+/-":
        if (displayValue.includes("-")) {
          displayValue = displayValue.substring(1);
          return;
        }
        displayValue = "-" + displayValue;
        break;
      default:
        console.log("Uuu");
    }
  };

  const numberClick = e => {
    if (resetDisplay) clearDisplay();
    if (displayValue.length >= 12) return;
    if (displayValue.includes(".") && e == ".") return;
    if (value == 0) {
      if (displayValue.includes(".")) {
        displayValue = displayValue + e;
        return;
      }
      if (e == "0") return;
      if (e == ".") {
        displayValue = displayValue + e;
        return;
      }
      displayValue = e;
      return;
    }
    displayValue = displayValue + e;
  };
</script>

<style>
  .container {
    display: flex;
    justify-content: center;
    padding: 20px;
  }
  .wrapper {
    color: white;
    background-color: rgb(34, 33, 35);
    display: inline-grid;
    grid-template-rows: 100px 80px 80px 80px 80px 80px;
    grid-template-columns: 100px 100px 100px 100px;
    grid-gap: 1px;
  }
  .grid-item {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 3em;
    cursor: pointer;
  }
  .display {
    grid-column: 1/5;
    display: flex;
    justify-content: flex-end;
    align-items: flex-end;
    font-size: 3em;
    padding: 20px;
  }
  .nula {
    grid-column: 1/3;
    display: flex;
    justify-content: flex-start;
    padding-left: 35px;
  }
  .orange-bg {
    background-color: rgb(253, 141, 14);
  }
  .grey-bg {
    background-color: rgb(78, 76, 80);
  }
  .grey-dark-bg {
    background-color: rgb(49, 49, 52);
  }
</style>

<div class="container">
  <div class="wrapper">
    <div class="display">{displayValue}</div>
    <div class="grid-item grey-dark-bg" on:click={() => functionClick('AC')}>
      AC
    </div>
    <div class="grid-item grey-dark-bg" on:click={() => functionClick('+/-')}>
      +/-
    </div>
    <div class="grid-item grey-dark-bg" on:click={() => operationClick('%')}>
      &percnt;
    </div>
    <div class="grid-item orange-bg" on:click={() => operationClick('/')}>
      &divide;
    </div>
    <div class="grid-item grey-bg" on:click={() => numberClick('7')}>7</div>
    <div class="grid-item grey-bg" on:click={() => numberClick('8')}>8</div>
    <div class="grid-item grey-bg" on:click={() => numberClick('9')}>9</div>
    <div class="grid-item orange-bg" on:click={() => operationClick('*')}>
      &times;
    </div>
    <div class="grid-item grey-bg" on:click={() => numberClick('4')}>4</div>
    <div class="grid-item grey-bg" on:click={() => numberClick('5')}>5</div>
    <div class="grid-item grey-bg" on:click={() => numberClick('6')}>6</div>
    <div class="grid-item orange-bg" on:click={() => operationClick('-')}>
      &minus;
    </div>
    <div class="grid-item grey-bg" on:click={() => numberClick('1')}>1</div>
    <div class="grid-item grey-bg" on:click={() => numberClick('2')}>2</div>
    <div class="grid-item grey-bg" on:click={() => numberClick('3')}>3</div>
    <div class="grid-item orange-bg" on:click={() => operationClick('+')}>
      &plus;
    </div>
    <div class="grid-item nula grey-bg" on:click={() => numberClick('0')}>
      0
    </div>
    <div class="grid-item grey-bg" on:click={() => numberClick('.')}>,</div>
    <div class="grid-item orange-bg" on:click={() => functionClick('=')}>
      &equals;
    </div>
  </div>
</div>
