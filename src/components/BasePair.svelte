<script lang="ts">
  import { mdiDelete } from "@mdi/js";
  import { getContext } from "svelte";
  import type { AppContextProps } from "../App.svelte";
  import { uppercase } from "../actions/uppercase.svelte";

  import Button from "./Button.svelte";
  import Icon from "./Icon.svelte";

  export let pair: string;
  export let index: number;

  let value = pair;

  $: updateSpecificPair(index, value);

  const { deleteSpecificPair, updateSpecificPair } =
    getContext<AppContextProps>("appContext");
</script>

<div class="base-pair">
  <input type="text" use:uppercase bind:value />

  <!-- delete -->
  <Button
    on:click={() => {
      deleteSpecificPair(index);
    }}
  >
    <Icon icon={mdiDelete} />
  </Button>
</div>

<style lang="scss">
  input {
    margin-bottom: 0;
    margin-right: 8px;
    width: 100%;
  }
  .base-pair {
    display: inline-flex;
  }
</style>
