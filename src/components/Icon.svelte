<script>
  export let size = "1.5em";
  export let width = size;
  export let height = size;
  export let color = "currentColor";
  export let viewBox = "0 0 24 24";
  export let icon = "";
</script>

<svg {width} {height} {viewBox}><path d={icon} fill={color} /></svg>
