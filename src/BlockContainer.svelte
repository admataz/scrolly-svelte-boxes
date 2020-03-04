<svelte:options tag="admataz-blocks-container" />

<script>
  let containerWidth = 0;
  let containerHeight = 0;
  let xlimit = 1;

  export let yoffset = 0;
  export let ylimit = 10;
  export let scale = 40;
  export let colours = ["#E86C4E", "#666", "#FFE074", "#71A166", "#667FCC"];

  function makeBlocks() {
    let blocks = [];
    let xpos;
    let ypos;
    let xoffset = 0;

    for (let y = 0; y < ylimit; y++) {
      for (let x = 0; x < xlimit; x++) {
        if (y && Math.random() < 0.7) {
          continue;
        }
        if (y % 2) {
          xoffset = scale / 2;
        } else {
          xoffset = 0;
        }

        xpos = x * scale - scale;
        ypos = (y * scale) / 4 - scale / 4;

        blocks.push({
          xpos,
          ypos,
          xoffset,
          fillColour: colours[Math.floor(Math.random() * colours.length)],
          randomizer: Math.ceil(Math.random() * 10)
        });
      }
    }
    return blocks;
  }

  $: blocksList =
    containerWidth && containerHeight && xlimit && ylimit ? makeBlocks(0) : [];
  $: xlimit = Math.ceil(containerWidth / scale) + 1;
  $: ylimit = Math.ceil((containerHeight / scale) * 4) + 1;
</script>

<style>

</style>

<div
  class="blocks-container"
  bind:clientWidth={containerWidth}
  bind:clientHeight={containerHeight}>
  <svg
    version="1.1"
    xmlns="http://www.w3.org/2000/svg"
    id="blocks-svg"
    viewBox={`0 0 ${containerWidth} ${ylimit}`}
    preserveAspectRatio="xMinYMin meet"
    overflow="visible"
    >
    {#each blocksList as itm}

      <g
      transform={`matrix( ${scale/4} 0 0 ${scale/4} ${itm.xpos + itm.xoffset * itm.randomizer} ${itm.ypos + yoffset * itm.randomizer})`}
      fill={itm.fillColour}
      stroke={itm.strokeColour}
      stroke-width={itm.strokeWidth}>
      <polygon points="2,2 0,1 2,0 4,1" fill={itm.fillColour} className="cubeTop" />
      <polygon
        points="2,2 2,4 0,3 0,1"
        fill={itm.fillColourFrontLeft}
        className="cubeFrontLeft" />
      <polygon
        points="2,2 4,1 4,3 2,4"
        fill={itm.fillColourFrontRight}
        className="cubeFrontRight" />
    </g>
    {/each}
  </svg>
</div>
