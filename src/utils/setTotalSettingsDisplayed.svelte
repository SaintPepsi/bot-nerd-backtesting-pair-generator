<script context="module" lang="ts">
  import { base_settings } from "../components/BotSetting.svelte";

  export function setTotalSettingsDisplayed(
    stopLossEnabled: boolean,
  ) {
    let root = document.documentElement;
    let totalSettings = Object.keys(base_settings).length;

    if (!stopLossEnabled) {
      totalSettings -= 1;
    }
    root.style.setProperty(
      "--total-settings-displayed",
      totalSettings.toString(),
    );
  }
</script>
