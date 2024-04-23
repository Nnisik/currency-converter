<script lang="ts">
  import "./style/style.css";

  // function changeOutputCurrency() {
  //   outputValue = exchangesRates.conversion_rates[selectOutputCurrency];
  // }
  
  function calculateFinalResult() {
    outputValue = inputValue * exchangesRates.conversion_rates[selectOutputCurrency]
  }

  function fetchAPI() {
    fetch(`https://v6.exchangerate-api.com/v6/${API_KEY}/latest/${selectInputCurrency}`,{method: "GET"})
      .then(response => response.json())
      .then(data => {
        // document.querySelector("#output")?.textContent = data.
        outputValue = data.conversion_rates[selectOutputCurrency];
        exchangesRates = data;
      });
    return;
  }

  // api key
  const API_KEY = "be3fe510a77785002265eff3";
  // current exchange rates
  let exchangesRates: { conversion_rates: { [x: string]: number; }; };
  let selectInputCurrency = 'USD';
  let selectOutputCurrency = 'USD';
  // $: selectOutputCurrency, changeOutputCurrency();
  const currencyOptions = [
    'USD',
    'EUR',
    'RUB'
  ];
  let inputValue: any;
  let outputValue: any;

  fetchAPI();
  // document.getElementById("input-currency")?.onchange = () => {
  //   
  // }
  // document.querySelector("#output")?.textContent = "";

</script>

<main>
  <article class="converter-container">
    <h1 class="converter-header">Convert currency</h1>
    <section class="converter-fields">
      <div class="currency-container" id="search">
        <input type="number" class="input" placeholder="1" id="input" bind:value={inputValue} />
        <select class="currensy-celect-btn" id="input-currency" bind:value={selectInputCurrency}>
          {#each currencyOptions as currency}
            <option {currency}>{currency}</option>
          {/each}
        </select>
      </div>
      <div class="currency-container" id="result">
        <p class="input" id="output">{outputValue}</p>
        <select class="currensy-celect-btn" id="result-currency" bind:value={selectOutputCurrency} >
          {#each currencyOptions as currency}
            <option {currency}>{currency}</option>
          {/each}
        </select>
      </div>
    </section>
  </article>
</main>