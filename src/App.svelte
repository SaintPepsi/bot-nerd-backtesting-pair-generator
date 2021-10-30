<script context="module" lang="ts">
  export interface AppContextProps {
    duplicateSetting(i: number): void;
    updateSpecificSettings(
      i: number,
      settings: BotSettingsProps,
    ): void;
  }
</script>

<script lang="ts">
  import { setContext } from "svelte";

  import BotSetting, {
    base_settings,
    BotSettingsProps,
  } from "./components/BotSetting.svelte";
  import { generateFinalCommand } from "./utils/generateFinalCommand.svelte";

  export let name: string;

  let resultTextFieldValue;

  let allSettings: Array<BotSettingsProps> = [base_settings];

  /**
   * Duplicate settings and add insert into next slot in array
   */
  function duplicateSetting(i: number) {
    console.log("i", i);
    const dupe = JSON.parse(JSON.stringify(allSettings[i]));
    allSettings.splice(i, 0, dupe);
    allSettings = allSettings;
  }

  /**
   * Update the main list of settings
   */
  function updateSpecificSettings(
    i: number,
    settings: BotSettingsProps,
  ) {
    allSettings[i] = settings;
    allSettings = allSettings;
  }

  const appContext = {
    duplicateSetting,
    updateSpecificSettings,
  };

  function outputFinalCommand() {
    const command = generateFinalCommand(allSettings);
    resultTextFieldValue = command;
  }

  setContext<AppContextProps>("appContext", appContext);
</script>

<svelte:head>
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

  <h3>Settings:</h3>

  {#each allSettings as settings, i}
    <BotSetting {settings} index={i} />
  {/each}

  <button on:click={outputFinalCommand}>Generate</button>
  <div class="result-command">
    <label for="result">Final Command</label>
    <textarea
      bind:value={resultTextFieldValue}
      name="result"
      id="result"
      cols="30"
      rows="10"
    />
  </div>
</main>

<style>
  main {
    width: 100%;
    max-width: 960px;
    margin: 0 auto;
  }
  #result {
    width: 100%;
  }
</style>
