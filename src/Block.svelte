<svelte:options tag="admataz-blocks-block" />
<script>
  export let colours = ["#E86C4E", "#666", "#FFE074", "#71A166", "#667FCC"];
  export let fillColour = colours[Math.floor(Math.random() * colours.length)];
  export let scale = 12;
  export let strokeColour = "#bbb";
  export let strokeWidth = 0.05;
  export let fillColourFrontLeft = "#fff";
  export let fillColourFrontRight = "#efefef";

  export let xpos = 0;
  export let ypos = 0;
  export let xoffset = 0;
  export let yoffset = 0;
  export let randomizer = 1;
  
  $: transform = `matrix( ${scale/4} 0 0 ${scale/4} ${xpos + xoffset * randomizer} ${ypos + yoffset * randomizer})`
</script>

<g
  {transform}
  fill={fillColour}
  stroke={strokeColour}
  stroke-width={strokeWidth}>
  <polygon points="2,2 0,1 2,0 4,1" fill={fillColour} className="cubeTop" />
  <polygon
    points="2,2 2,4 0,3 0,1"
    fill={fillColourFrontLeft}
    className="cubeFrontLeft" />
  <polygon
    points="2,2 4,1 4,3 2,4"
    fill={fillColourFrontRight}
    className="cubeFrontRight" />
</g>
