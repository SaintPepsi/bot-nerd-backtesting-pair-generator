<script context="module" lang="ts">
  export interface AppContextProps {
    duplicateSetting(i: number): void;
    updateSpecificSettings(
      i: number,
      settings: BotSettingsProps,
    ): void;
    updateSpecificPair(i: number, value: string): void;
    deleteSpecificSettings(i: number): void;
    deleteSpecificPair(i: number): void;
  }
</script>

<script lang="ts">
  import dayjs from "dayjs";
  import { Datepicker } from "svelte-calendar";
  import { setContext } from "svelte";
  import { mdiFileCodeOutline } from "@mdi/js";
  import { mdiPlusThick } from "@mdi/js";
  import { mdiContentCopy } from "@mdi/js";
  import { mdiGithub } from "@mdi/js";
  import { mdiCheckboxMarked } from "@mdi/js";
  import { mdiCheckboxBlankOutline } from "@mdi/js";

  import localforage from "localforage";

  import BotSetting, {
    base_settings,
    BotSettingsProps,
  } from "./components/BotSetting.svelte";
  import { generateFinalCommand } from "./utils/generateFinalCommand.svelte";
  import { calendarTheme } from "./data/calendarTheme";
  import Button from "./components/Button.svelte";
  import Icon from "./components/Icon.svelte";
  import BasePair from "./components/BasePair.svelte";
  import { uppercase } from "./actions/uppercase.svelte";
  import { setStorageEntry } from "./utils/StorageManager.svelte";

  export let name: string;

  let resultTextFieldValue: string;

  let allSettings: Array<BotSettingsProps> = [base_settings];
  localforage
    .getItem("allSettings")
    .then((value: Array<BotSettingsProps>) => {
      if (
        Object.keys(allSettings[0].length) ===
        Object.keys(value[0].length)
      ) {
        allSettings = value;
      }
    });

  let allBasePairs: Array<string> = ["BTC"];
  localforage.getItem("allBasePairs").then((value: Array<string>) => {
    allBasePairs = value;
  });

  let quotePair = "USD";
  localforage.getItem("quotePair").then((value: string) => {
    quotePair = value;
  });

  let dateStoreStartDate: any;
  let dateStoreEndDate: any;

  let stopLossEnabled = true;
  localforage.getItem("stopLossEnabled").then((value: boolean) => {
    stopLossEnabled = value;
  });

  $: startDate = dayjs($dateStoreStartDate?.selected).format(
    "MM/DD/YYYY",
  );
  $: endDate = dayjs($dateStoreEndDate?.selected).format(
    "MM/DD/YYYY",
  );

  $: setStorageEntry("quotePair", quotePair);
  /**
   * Duplicate settings and add insert into next slot in array
   */
  function duplicateSetting(i: number) {
    console.log("i", i);
    console.log("allSettings[i]", allSettings[i]);
    const dupe = JSON.parse(JSON.stringify(allSettings[i]));
    allSettings.splice(i, 0, dupe);
    allSettings = allSettings;
    setStorageEntry("allSettings", allSettings);
  }

  // Delete Functions
  function deleteSpecificSettings(i: number) {
    if (allSettings.length <= 1) return;
    allSettings.splice(i, 1);
    allSettings = allSettings;
    setStorageEntry("allSettings", allSettings);
  }

  function deleteSpecificPair(i: number) {
    if (allBasePairs.length <= 1) return;
    allBasePairs.splice(i, 1);
    allBasePairs = allBasePairs;
    setStorageEntry("allBasePairs", allBasePairs);
  }

  // Update functions
  function updateSpecificSettings(
    i: number,
    settings: BotSettingsProps,
  ) {
    allSettings[i] = settings;
    allSettings = allSettings;
    setStorageEntry("allSettings", allSettings);
  }

  function addNewPair() {
    allBasePairs = [...allBasePairs, ""];
    setStorageEntry("allBasePairs", allBasePairs);
  }

  function updateSpecificPair(i: number, value: string) {
    allBasePairs[i] = value;
    allBasePairs = allBasePairs;
    setStorageEntry("allBasePairs", allBasePairs);
  }

  const appContext = {
    duplicateSetting,
    updateSpecificSettings,
    deleteSpecificSettings,
    deleteSpecificPair,
    updateSpecificPair,
  };

  function outputFinalCommand() {
    const command = generateFinalCommand(
      allSettings,
      startDate,
      endDate,
      allBasePairs,
      quotePair,
      stopLossEnabled,
    );
    resultTextFieldValue = command;
  }

  setContext<AppContextProps>("appContext", appContext);

  let powercommandTextArea;
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
  <div class="section">
    <h2>How to use:</h2>
    <h3>1. Add Base Pairs</h3>
    <h3>2. Change Quote Pair</h3>
    <h3>3. Select Start / End Date</h3>
    <h3>4. Add / Remove Settings</h3>
    <h3>4. Generate Backtest Command</h3>
    <small
      >*Automatically saves current settings to local storage</small
    >
  </div>

  <div class="base-pair-wrapper section">
    <p>Base pairs</p>

    <div class="pair-wrapper">
      {#each allBasePairs as basePair, i}
        <BasePair pair={basePair} index={i} />
      {/each}
      <!-- Add -->
      <Button
        on:click={() => {
          addNewPair();
        }}
      >
        <Icon icon={mdiPlusThick} />
      </Button>
    </div>
  </div>
  <div class="section">
    <div class="quote-pair">
      <p>Quote pair</p>

      <input type="text" bind:value={quotePair} use:uppercase />
    </div>
  </div>

  <div class="date-pickers section">
    <div class="date-wrapper">
      <h3>Start Date</h3>
      <Datepicker
        bind:store={dateStoreStartDate}
        theme={calendarTheme}
      />
    </div>
    <div class="date-wrapper">
      <h3>End Date</h3>
      <Datepicker
        bind:store={dateStoreEndDate}
        theme={calendarTheme}
      />
    </div>
  </div>

  <div class="section">
    <div class="vetical-flex-space">
      <h3>Settings:</h3>

      <Button
        on:click={() => {
          stopLossEnabled = !stopLossEnabled;
          setStorageEntry("stopLossEnabled", stopLossEnabled);
        }}
      >
        <Icon
          icon={stopLossEnabled
            ? mdiCheckboxMarked
            : mdiCheckboxBlankOutline}
          slot="icon"
        />
        Stop Loss
      </Button>
    </div>
    {#each allSettings as settings, i}
      <BotSetting {settings} {stopLossEnabled} index={i} />
    {/each}
  </div>

  <div class="section">
    <div class="result-command">
      <label for="result">Final Command</label>
      <textarea
        bind:value={resultTextFieldValue}
        bind:this={powercommandTextArea}
        name="result"
        id="result"
        class="commmand-textarea"
        rows="5"
      />
    </div>

    <Button on:click={outputFinalCommand}>
      <Icon icon={mdiFileCodeOutline} slot="icon" />
      Generate
    </Button>

    <Button
      on:click={() => {
        navigator.clipboard
          .writeText(resultTextFieldValue)
          .then(() => {
            // Success!
          })
          .catch((err) => {
            console.log("Something went wrong", err);
          });
      }}
    >
      <Icon icon={mdiContentCopy} slot="icon" />
      Copy power command
    </Button>
  </div>

  <div class="section">
    <a href="https://www.buymeacoffee.com/SanCoca" target="_blank"
      ><img
        src="https://cdn.buymeacoffee.com/buttons/default-yellow.png"
        alt="Buy Me A Coffee"
        height="41"
        width="174"
      /></a
    >
  </div>

  <div class="section bug-report">
    <h3>Bug?</h3>
    <small>@SanCoca#4418 on discord - catch me on [bot nerds]</small>
    <a
      href="https://github.com/SaintPepsi/bot-nerd-backtesting-pair-generator"
      target="_blank"
    >
      <Button>
        <Icon icon={mdiGithub} slot="icon" />
        View on github
      </Button>
    </a>
  </div>
</main>

<style lang="scss">
  @import "./styles/button";
  main {
    width: 100%;
    max-width: 960px;
    margin: 0 auto;
    padding-top: 16px;
    padding-bottom: 16px;
  }
  #result {
    width: 100%;
  }

  .bug-report {
    small,
    a {
      margin-top: 8px;
      display: block;
    }
  }
  .vetical-flex-space {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 8px;
    margin-bottom: 8px;
  }
  .pair-wrapper {
    display: flex;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 8px;
    @media screen and (max-width: 1000px) {
      grid-template-columns: repeat(3, 1fr);
    }
    @media screen and (max-width: 767px) {
      grid-template-columns: repeat(2, 1fr);
    }
    @media screen and (max-width: 500px) {
      grid-template-columns: repeat(1, 1fr);
    }
  }

  .quote-pair {
    @media screen and (max-width: 767px) {
      input {
        width: 100%;
      }
    }
  }
  .section {
    margin-bottom: 32px;
  }
  .date-pickers {
    display: flex;
    justify-content: space-evenly;
    @media screen and (max-width: 767px) {
      display: block;
      .date-wrapper {
        &:last-child {
          margin-top: 24px;
        }
        :global(.trigger) {
          width: calc(100vw - 16px);
        }
      }
    }

    .date-wrapper {
      display: flex;
      align-items: center;
      flex-direction: column;
      h3 {
        margin-bottom: 16px;
      }
      :global(.sc-popover .trigger) {
        @extend .base-button;
      }
      :global(.sc-popover .trigger:hover) {
        @extend .base-button-hover;
      }
      :global(.sc-popover .trigger .button-container button) {
        box-shadow: none;
        background-color: transparent;
      }
    }
  }

  :global(.sc-popover .contents-wrapper) {
    overflow: visible !important;
  }
</style>
