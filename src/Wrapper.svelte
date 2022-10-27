<script>
  import Blotch from "./Blotch/index.svelte";

  let speed = 1;
  let volatility = 0.001;

  const speedRange = { min: 0, max: 15 };
  let volatilityRange = { min: 0.0001, max: 0.5 };

  const config = {
    family: "Chalkduster, serif",
    size: 87,
    fill: "#171717",
    paddingLeft: 40,
    paddingRight: 40,
  };

  // ☢️ The [docs](https://blotter.js.org/#/basics) are out of date, apparently. LiquidDistortMaterial takes neither `new` nor `()`!

  // Similarly, both uXXXXXX values are over-described in the docs.

  let material = new Blotter.LiquidDistortMaterial();
  $: {
    material.uniforms.uSpeed.value = speed;
    material.uniforms.uVolatility.value = volatility;
  }

  // Baseline (still water) S1 V.001
  // Splash S15 V.5
  // Rainfall S7 V.25
  // Harbor S1 V.5
  // Wineglass S15 V.001

  /*
    Start: Baseline
    On click: change()

    change: Splash -> Baseline
    timeTillNextChange: randomIntFromInterval(3,20)
  */

  // https://stackoverflow.com/questions/4959975/generate-random-number-between-two-numbers-in-javascript
  function randomIntFromInterval(min, max) {
    return Math.random() * (max - min + 1) + min;
  }

  let nextInterval = 2000;

  const change = (initial = true) => {
    if (initial) {
      speed = speedRange.max;
      volatility = volatilityRange.max;
    } else {
      speed = speed - (speedRange.max - speedRange.min) / 10;
      volatility =
        volatility - (volatilityRange.max - volatilityRange.min) / 10;
    }
    nextInterval = randomIntFromInterval(1000, 3000);
    if (speed >= speedRange.min) {
      setTimeout(() => {
        change(false);
      }, nextInterval);
    } else {
      speed = speedRange.min;
      volatility = volatilityRange.min;
    }
  };
  change();
</script>

{nextInterval}
{speed >= speedRange.min}
{material.uniforms.uSpeed.value}
<Blotch text="Default Blotch" />
<Blotch text="Still" {config} {material} />

<div>
  <label for="speed">Speed</label>
  <input
    name="speed"
    type="range"
    bind:value={speed}
    min={speedRange.min}
    max={speedRange.max}
    step="0.1"
  />
  <output>{speed}/{speedRange.max}</output>
</div>

<div>
  <label for="volatility">Volatility</label>
  <input
    name="volatility"
    type="range"
    bind:value={volatility}
    min={volatilityRange.min}
    max={volatilityRange.max}
    step="0.001"
  />
  <input
    name="volatility"
    type="number"
    bind:value={volatility}
    min={volatilityRange.min}
    max={volatilityRange.max}
    step="0.001"
  />
  <output>/{volatilityRange.max}</output>
</div>

<style>
  div {
    display: flex;
  }
  div label,
  div input,
  div output {
    padding: 1rem;
    display: inline-block;
    vertical-align: baseline;
  }
</style>
