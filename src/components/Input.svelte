<script lang="ts">
  import { getContext } from "svelte";
  import type { UpdateSettingsProps } from "./BotSetting.svelte";

  export let setting: string;
  export let alternativeHoverText: string;
  $: id = `setting-${setting}`;

  export let value: number;

  const updateSettings =
    getContext<UpdateSettingsProps>("updateSettings");
  $: updateSettings(setting, value);
</script>

<div class="settings">
  <label for={id}>
    {setting.toUpperCase()}
  </label>
  <input type="number" {id} bind:value />
</div>

<style type="text/scss">
  @import "../styles/infotext";
  .settings {
    position: relative;
    z-index: 1;
    label {
      white-space: nowrap;
    }
    input {
      width: 100%;
      z-index: 1;
    }
  }
</style>
