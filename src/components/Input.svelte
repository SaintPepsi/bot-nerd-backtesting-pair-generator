<script lang="ts">
  import { getContext } from "svelte";
  import type { UpdateSettingsProps } from "./BotSetting.svelte";

  export let setting: string;

  $: id = `setting-${setting}`;

  export let value: number;

  const updateSettings =
    getContext<UpdateSettingsProps>("updateSettings");
  $: updateSettings(setting, value);
</script>

<div class="settings">
  <label for={id}>{setting.toUpperCase()}</label>
  <input type="number" {id} bind:value />
</div>

<style type="text/scss">
  .settings {
    input {
      width: 100%;
    }
  }
</style>
