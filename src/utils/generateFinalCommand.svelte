<script context="module" lang="ts">
  import {
    BotSettingsProps,
    settingsKeys,
    totalSettingProps,
  } from "../components/BotSetting.svelte";

  export function generateFinalCommand(
    all_settings: Array<BotSettingsProps>,
    startDate: string,
    endDate: string,
    basePairs: Array<string>,
    quotePair: string,
    stopLossEnabled: boolean,
  ) {
    const pairs = generatePairs(basePairs, quotePair);

    let finalCommand = `!dcabacktest -pair ${pairs} `;

    for (let i = 0; i < totalSettingProps; i++) {
      let setting = settingsKeys[i];

      // If stoploss is not enable don't add it to final command
      if (setting === "sl" && !stopLossEnabled) continue;

      finalCommand += `-${setting} `;
      all_settings.forEach((settings, j) => {
        let value = settings[setting];
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

  function generatePairs(
    basePairs: Array<string>,
    quotePair: string,
  ) {
    let allPairs = "";
    basePairs.forEach((pair, i) => {
      allPairs += `${pair}/${quotePair}`;
      if (i < basePairs.length - 1) {
        allPairs += ", ";
      }
    });
    return allPairs;
  }
</script>
