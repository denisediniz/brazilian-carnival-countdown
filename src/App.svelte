<script>
  import Countdown from "./Countdown.svelte";
  import Confetti from "./Confetti.svelte";
</script>

<style>
  :global(body) {
    overflow: hidden;
    background-color: black;
    color: white;
    padding: 0;
    text-transform: uppercase;
  }
  main {
    display: grid;
    width: 100vw;
    height: 100vh;
    place-items: center center;
  }
</style>

<Confetti/>
<main>
  <Countdown title="Brazilian Carnival Countdown" deadline="Feb 25, 2020 00:00:00" />
</main>
