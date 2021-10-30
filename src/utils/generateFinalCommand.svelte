<script context="module" lang="ts">
  import {
    BotSettingsProps,
    settingsKeys,
    totalSettingProps,
  } from "../components/BotSetting.svelte";

  export function generateFinalCommand(
    all_settings: Array<BotSettingsProps>,
  ) {
    const pair = "ETH/BTC";
    const startDate = "28/10/2020";
    const endDate = "27/10/2021";

    let finalCommand = `!dcabacktest -pair ${pair} `;

    for (let i = 0; i < totalSettingProps; i++) {
      finalCommand += `-${settingsKeys[i]} `;
      all_settings.forEach((settings, j) => {
        let value = settings[settingsKeys[i]];
        // Add the setting value to the command.
        finalCommand += value;
        if (j < all_settings.length - 1) {
          // If there's more than 1 settings add a comma.
          finalCommand += ",";
        } else {
          // if it's the last one add a space.
          finalCommand += " ";
        }
      });
    }

    finalCommand += `-sd ${startDate} -ed ${endDate}`;

    console.log("finalCommand", finalCommand);
    return finalCommand;
  }
</script>
