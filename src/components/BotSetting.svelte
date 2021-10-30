<script context="module" lang="ts">
  export interface BotSettingsProps {
    [key: string]: number;
  }
  // Base Settings
  export const base_settings: BotSettingsProps = {
    tp: 1,
    bo: 10,
    so: 10,
    mstc: 5,
    sos: 1,
    ss: 1,
    fees: 0.075,
  };
  export const settingsKeys = Object.keys(base_settings);
  export const totalSettingProps = settingsKeys.length;

  let root = document.documentElement;
  root.style.setProperty(
    "--total-settings",
    totalSettingProps.toString(),
  );

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
</script>

<!-- Multiple of these. -->
<div class="settings-wrapper">
  <!-- Drag to rearrange -->
  <div class="bot-settings">
    {#each Object.entries(settings) as [key, value]}
      <Input setting={key} {value} />
    {/each}
  </div>

  <!-- TODO: delete -->
  <Button
    on:click={() => {
      duplicateSetting(index);
    }}
  >
    <Icon icon={mdiContentCopy} />
  </Button>

  <!-- Duplicate current settings -->
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
      margin-right: 16px;
    }
    :global(.button:last-child) {
      margin-right: 0;
    }
    margin-bottom: 16px;
  }
  .bot-settings {
    display: grid;
    grid-template-columns: repeat(var(--total-settings), 1fr);
    gap: 16px;
    margin-right: 16px;
  }
</style>
