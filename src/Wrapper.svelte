<script>
  import Blotch from "./Blotch/index.svelte";

  const config = {
    family: "'EB Garamond', serif",
    size: 87,
    fill: "#171717",
    paddingLeft: 40,
    paddingRight: 40,
  };

  // ☢️ The [docs](https://blotter.js.org/#/basics) are out of date, apparently. LiquidDistortMaterial takes neither `new` nor `()`!

  // Similarly, both uXXXXXX values are over-described in the docs.

  console.log(Blotter);
  let material = new Blotter.LiquidDistortMaterial();

  material.uniforms.uSpeed.value = 0.3;
  material.uniforms.uVolatility.value = 0.1;
  material.uniforms.uSeed.value = 0.1;
</script>

<Blotch text="Hello me!" />
<Blotch text="Bye me. 😥" {config} {material} />
