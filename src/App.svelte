<script>
  import localForage from "localforage";

  export let name;
</script>

<svelte:head>
  <!-- Material Icons -->
  <link
    rel="stylesheet"
    href="https://fonts.googleapis.com/icon?family=Material+Icons"
  />
  <!-- Roboto -->
  <link
    rel="stylesheet"
    href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,600,700"
  />
  <!-- Roboto Mono -->
  <link
    rel="stylesheet"
    href="https://fonts.googleapis.com/css?family=Roboto+Mono"
  />
  <link
    rel="stylesheet"
    href="./node_modules/svelte-material-ui/bare.css"
  />
</svelte:head>

<main>
  <h1>Hello {name}! Bot Tester</h1>

  <div class="base-pair">
    <p>Base pair</p>
    <ul>
      <li>BTC</li>
      <li>ETH</li>
      <li>SOL</li>
    </ul>
  </div>

  <div class="quote-pair">
    <p>Quote pair</p>
    <ul>
      <li>USDT</li>
      <li>BUSD</li>
    </ul>
  </div>

  <div class="date">CALENDAR PICKER START/END DATE</div>

  <!-- Multiple of these. -->
  <div class="bot-settings">
    <span>TP: 3</span>
    <span>BO: 10</span>
    <span>SO: 10</span>
    <span>MSTC: 10</span>
    <span>SOS: 10</span>
    <span>OS: 10</span>
    <span>SS: 10</span>
    <span>FEES: 0.75</span>
  </div>

  <div class="result-command">
    <label for="result">Final Command</label>
    <textarea name="result" id="result" cols="30" rows="10" />
  </div>
</main>

<style>
</style>
