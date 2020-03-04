<script>
  import { onMount, tick } from "svelte";
  function onScroll(evt) {
    yoffset = evt.target.scrollTop;
  }

  export let containerwidth = 10;
  export let containerheight = 960;
  let xlimit = 1;

  export let yoffset = 0;
  export let ylimit = 10;
  export let scale = 20;
  export let colours = ["#E86C4E", "#666", "#FFE074", "#71A166", "#667FCC"];
  export let strokecolour = "#bbb";
  export let strokewidth = 0.05;
  export let fillcolourfrontleft = "#fff";
  export let fillcolourfrontright = "#efefef";
  export let gapfrequency = 0.7;
  export let definedylimit = 0;
  export let randomness = 1;

  function makeBlocks() {
    let blocks = [];
    let xpos;
    let ypos;
    let xoffset = 0;

    for (let y = 0; y < ylimit; y++) {
      for (let x = 0; x < xlimit; x++) {
        if (y && Math.random() < gapfrequency) {
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
          randomizer: Math.random() * randomness
        });
      }
    }
    return blocks;
  }



  $: xlimit = Math.ceil(containerwidth / scale) + 1;
  $: blocksList = containerwidth && containerheight ? makeBlocks(0) : [];
  $: ylimit = definedylimit || Math.ceil((containerheight / scale) * 4) + 1;
  $: console.log({ xlimit, containerwidth, containerheight });
</script>

<style>
  .scrolling-block-container {
    width: 100%;
    height: 100%;
    position: absolute;
  }
  .bg {
    position: absolute;
    width: 100%;
    height: 100%;
  }
  .fg {
    position: absolute;
    overflow: auto;
    margin: 0 auto;
    width: 100%;
    height: 100%;
  }
  .blocks-container {
    top: "0";
    height: 100%;
    width: 100%;
  }
  #blocks-svg {
    width: 100%;
    height: 100%;
    position: absolute;
  }
</style>

<svelte:options tag="admataz-blocks" />
<div class="scrolling-block-container">
  <div class="bg">
    <div class="blocks-container">
      <svg
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        id="blocks-svg"
        viewBox={`0 0 ${containerwidth} ${ylimit}`}
        preserveAspectRatio="xMinYMin meet">
        {#each blocksList as itm}
          <g
            transform={`matrix( ${scale / 4} 0 0 ${scale / 4} ${itm.xpos + itm.xoffset} ${itm.ypos + yoffset * itm.randomizer})`}
            fill={itm.fillColour}
            stroke={strokecolour}
            stroke-width={strokewidth}>
            <polygon
              points="2,2 0,1 2,0 4,1"
              fill={itm.fillColour}
              className="cubeTop" />
            <polygon
              points="2,2 2,4 0,3 0,1"
              fill={fillcolourfrontleft}
              className="cubeFrontLeft" />
            <polygon
              points="2,2 4,1 4,3 2,4"
              fill={fillcolourfrontright}
              className="cubeFrontRight" />
          </g>
        {/each}
      </svg>
    </div>
  </div>

  <div
    class="fg"
    on:scroll={onScroll}>
    <slot />
  </div>

</div>
