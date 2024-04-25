<script lang="ts">
  import "./style/style.css";

  function calculateInputValue() {
    inputValue = (outputValue / exchangesRates.conversion_rates[selectInputCurrency]).toFixed(3);
  }
  
  function calculateOutputValue() {
    outputValue = (exchangesRates.conversion_rates[selectOutputCurrency] * inputValue).toFixed(3);
  }

  function changeInputCurrancy() {
    calculateInputValue();
  }

  function changeOutputCurrancy() {
    outputValue = exchangesRates.conversion_rates[selectOutputCurrency];
    calculateOutputValue();
  }

  function fetchAPI() {
    fetch(
      `https://v6.exchangerate-api.com/v6/${API_KEY}/latest/${selectInputCurrency}`,
      { method: "GET" }
    )
      .then((response) => response.json())
      .then((data) => {
        outputValue = data.conversion_rates[selectOutputCurrency];
        exchangesRates = data;
      });
    return;
  }

  // api key
  const API_KEY = "be3fe510a77785002265eff3";
  // current exchange rates
  let exchangesRates: { conversion_rates: { [x: string]: any; }; };
  let selectInputCurrency = "USD";
  let selectOutputCurrency = "EUR";
  // $: selectOutputCurrency, changeOutputCurrency();
  const currencyOptions = ["USD", "EUR", "CHF", "GBP", "CNY", "JPY", "RUB"];
  let inputValue: any = 1;
  let outputValue: any;
  
  fetchAPI();

</script>

<main>
  <article class="converter-container">
    <h1 class="converter-header">Convert currency</h1>
    <section class="converter-fields">
      <div class="currency-container" id="search">
        <input
          type="number"
          class="input"
          placeholder="1"
          id="input"
          bind:value={inputValue}
          on:change={calculateOutputValue}
        />
        <select
          class="currensy-celect-btn"
          id="input-currency"
          bind:value={selectInputCurrency}
          on:change={changeInputCurrancy}
        >
          {#each currencyOptions as currency}
            <option {currency}>{currency}</option>
          {/each}
        </select>
      </div>
      <div class="currency-container" id="result">
        <input
          type="number"
          class="input"
          id="output"
          bind:value={outputValue}
          on:change={calculateInputValue}
        />
        <select
          class="currensy-celect-btn"
          id="result-currency"
          bind:value={selectOutputCurrency}
          on:change={changeOutputCurrancy}
        >
          {#each currencyOptions as currency}
            <option {currency}>{currency}</option>
          {/each}
        </select>
      </div>
    </section>
  </article>
</main>
