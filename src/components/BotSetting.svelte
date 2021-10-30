<script context="module" lang="ts">
  export interface BotSettingsProps {
    [key: string]: number;
  }
  export interface BotSettingsInfoProps {
    [key: string]: string;
  }
  // Base Settings
  export const base_settings: BotSettingsProps = {
    tp: 1,
    bo: 10,
    so: 10,
    mstc: 5,
    sos: 1,
    os: 1,
    ss: 1,
    sl: 1,
    fees: 0.075,
  };

  export const base_settings_info: BotSettingsInfoProps = {
    tp: "Take Profit",
    bo: "Buy Order",
    so: "Safety Order",
    mstc: "Max Safety Trade Count",
    sos: "Safety Order Size",
    os: "Order Scale",
    ss: "Safety Scale",
    sl: "Stop Loss",
    fees: "Fees percentage per trade",
  };

  export const settingsKeys = Object.keys(base_settings);
  export const totalSettingProps = settingsKeys.length;

  export type UpdateSettingsProps = (
    key: string,
    value: number,
  ) => void;
</script>

<script lang="ts">
  import { mdiContentCopy } from "@mdi/js";
  import { mdiDelete } from "@mdi/js";

  import { getContext, setContext } from "svelte";
  import type { AppContextProps } from "../App.svelte";
  import Button from "./Button.svelte";
  import Icon from "./Icon.svelte";

  import Input from "./Input.svelte";

  export let settings: BotSettingsProps = {};
  export let stopLossEnabled = false;

  export let index: number;
  const {
    duplicateSetting,
    updateSpecificSettings,
    deleteSpecificSettings,
  } = getContext<AppContextProps>("appContext");

  function updateSettings(key: string, value: number) {
    settings[key] = value;
    console.log("settings", settings);
    updateSpecificSettings(index, settings);
  }

  setContext<UpdateSettingsProps>("updateSettings", updateSettings);

  $: filteredSettings = Object.entries(settings).filter(
    ([key, value]) => {
      if (key !== "sl") {
        return true;
      } else {
        if (stopLossEnabled) {
          return true;
        }
      }
      return false;
    },
  );
</script>

<!-- Multiple of these. -->
<div class="settings-wrapper">
  <!-- Drag to rearrange -->
  <div class="bot-settings">
    {#each filteredSettings as [key, value]}
      <Input
        setting={key}
        {value}
        alternativeHoverText={base_settings_info[key]}
      />
    {/each}
  </div>

  <!-- Duplicate current settings -->
  <Button
    on:click={() => {
      duplicateSetting(index);
    }}
  >
    <Icon icon={mdiContentCopy} />
  </Button>

  <!-- TODO: delete -->
  <Button
    on:click={() => {
      deleteSpecificSettings(index);
    }}
  >
    <Icon icon={mdiDelete} />
  </Button>
</div>

<style type="text/scss">
  .settings-wrapper {
    display: flex;
    justify-content: space-between;
    align-items: center;

    :global(.button) {
      align-self: stretch;
      margin-right: 8px;
    }
    :global(.button:last-child) {
      margin-right: 0;
    }
    margin-bottom: 16px;
  }
  .bot-settings {
    display: flex;
    gap: 8px;
    margin-right: 8px;
    @media screen and (max-width: 767px) {
      grid-template-columns: repeat(3, 1fr);
    }
  }
</style>
