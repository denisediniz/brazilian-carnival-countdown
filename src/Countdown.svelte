<script>
  import { onMount } from "svelte";
  import { fly } from "svelte/transition";
  import { quintOut } from "svelte/easing";

  /**
   * Countdown component
   * @param deadline - set a final date
   * @param title - set the component title
   * Countdow reference {@link https://codepen.io/AllThingsSmitty/pen/JJavZN?editors=0010}
   */
  export let deadline, title;

  const second = 1000,
    minute = second * 60,
    hour = minute * 60,
    day = hour * 24,
    endDate = new Date(deadline).getTime();

  let seconds, minutes, hours, days, diffDates;

  let leadingZero = element => {
    return (element < 10 ? "0" : "") + element;
  };

  onMount(() => {
    if (diffDates <= 0) clearInterval(countdownInterval);
    
    let countdownInterval = setInterval(() => {
      let startDate = new Date().getTime();

      diffDates = endDate - startDate;

      days = leadingZero(Math.floor(diffDates / day));
      hours = leadingZero(Math.floor((diffDates % day) / hour));
      minutes = leadingZero(Math.floor((diffDates % hour) / minute));
      seconds = leadingZero(Math.floor((diffDates % minute) / second));
    }, second);

    return () => clearInterval(countdownInterval);
  });
</script>

<style>
  .countdown {
    text-align: center;
  }
  .countdown__title {
    font-size: 1.5rem;
  }
  .countdown__wrapper {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }
  .countdown__item {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    padding: 2rem;
  }
  .countdown__value {
    font-size: 3rem;
  }
  @media all and (orientation: portrait) {
    .countdown__item {
      padding: 1rem;
    }
  }
  @media all and (orientation: landscape) {
    .countdown__wrapper {
      flex-direction: row;
    }
  }
  @media (min-width: 768px) {
    .countdown__wrapper {
      flex-direction: row;
    }
    .countdown__title {
      font-size: 2rem;
    }
    .countdown__value {
      font-size: 8rem;
    }
  }
  @media (min-width: 992px) {
    .countdown__value {
      font-size: 10rem;
    }
  }
</style>

{#if days !== undefined}

<div class="countdown">
  <h1 class="countdown__title" in:fly="{{y: 200, duration: 500}}">
    {title}
  </h1>
  {#if diffDates <= 0}
  <div
    in:fly="{{delay: 500, y: 200, duration: 500}}"
    class="countdown__wrapper"
  >
    <div class="countdown__item">
      <div class="countdown__value">Party time!</div>
    </div>
  </div>
  {:else}
  <div
    in:fly="{{delay: 500, y: 200, duration: 500}}"
    class="countdown__wrapper"
  >
    <div class="countdown__item">
      <div class="countdown__value">{days}</div>
      <span class="countdown__span">days</span>
    </div>
    <div class="countdown__item">
      <div class="countdown__value">{hours}</div>
      <span class="countdown__span">hours</span>
    </div>
    <div class="countdown__item">
      <div class="countdown__value">{minutes}</div>
      <span class="countdown__span">minutes</span>
    </div>
    <div class="countdown__item">
      <div class="countdown__value">{seconds}</div>
      <span class="countdown__span">seconds</span>
    </div>
  </div>
  {/if}
</div>
{/if}
