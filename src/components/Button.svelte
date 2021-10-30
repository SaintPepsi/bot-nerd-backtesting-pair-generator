<script lang="ts">
</script>

<div class="button" on:click>
  {#if $$slots.icon}
    <div class="icon-slot">
      <slot name="icon" />
    </div>
  {/if}
  <slot />
</div>

<style type="text/scss">
  @import "../styles/button";
  .button {
    @extend .base-button;
    .icon-slot {
      margin-left: -8px;
      margin-right: 8px;
      display: flex;
      align-items: center;
    }
  }
</style>
