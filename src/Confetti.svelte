<script>
  import { onMount } from "svelte";
  /**
   * Create and fill all array elements
   * @property color set a random scale and a random position for each confetti from 0 to 99
   * @property top value is negative to improve the initial animation effect
   * @property left set a random number between two values: Math.random() * (max - min) + min
   * Random color reference {@link https://codegolf.stackexchange.com/questions/12563/random-css-color-code/12565#12565}.
   */
  let confettiPile = new Array(50).fill().map(confetti => {
    let elementScale = Math.random() * (3 - 1) + 1;
    return {
      color: `#${Math.random()
        .toString(16)
        .substr(2, 6)}`,
      scale: elementScale,
      top: -Math.floor(Math.random() * 100),
      left: Math.floor(Math.random() * 100),
      zIndex: Math.floor(elementScale * 10)
    };
  });

  // Set an animated top position recursively
  let animateTop;
  let loop = () => {
    confettiPile = confettiPile.map(confetti => {
      // Increment top value on each frame (60 fps), scale gives an unique speed value on each confetti
      confetti.top += 0.2 * confetti.scale;
      if (confetti.top > 100) confetti.top = 0;
      return confetti;
    });
    animateTop = requestAnimationFrame(loop);
  };

  onMount(() => {
    // Run loop function as soon as the component has been mounted to the DOM
    loop();
    // If a function is returned from onMount, it will be called when the component is unmounted.
    return () => cancelAnimationFrame(animateTop);
  });
</script>

<style>
  .confetti{
    position: absolute;
    overflow: hidden;
    width: 100vw;
    height: 100vh;
    top: 0;
    left: 0;
  }
  .confetti__item {
    position: absolute;
    width: 1rem;
    height: 1rem;
    border-radius: 50%;
    background-color: black;
  }
</style>

<div class="confetti">
{#each confettiPile as confetti}
<span
  class="confetti__item"
  style="background-color: {confetti.color}; top: {confetti.top}%; left: {confetti.left}%; z-index: {confetti.zIndex}; transform: scale({confetti.scale});"
/>
{/each}
</div>
