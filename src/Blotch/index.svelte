<script>
  // import Blotter from "./blotter.min.js";
  console.log(Blotter);

  export let text = "Blotter Default Text ✒️";
  export let material = new Blotter.Material();
  export let config = {
    family: "'EB Garamond', serif",
    size: 57,
    fill: "#202020",
  };

  let elem;

  $: texts = new Blotter.Text(text, config);
  $: blotter = new Blotter(material, { texts });
  $: scope = blotter.forText(texts);
  $: if (elem) scope.appendTo(elem);
</script>

<div bind:this={elem} />
