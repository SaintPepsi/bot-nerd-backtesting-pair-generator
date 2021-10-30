<script context="module" lang="ts">
  import localforage from "localforage";
  export function setStorageEntry(key: string, value: any) {
    localforage
      .setItem(key, value)
      .then(function (value) {
        // Do other things once the value has been saved.
        console.log("sotrage succes:", key);
      })
      .catch(function (err) {
        // This code runs if there were any errors
        console.log(err);
      });
  }
</script>
