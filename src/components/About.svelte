<script>
  import FluidSim from "./FluidSim.svelte";

  import { onDestroy, onMount } from "svelte";
  let observer, sect;
  let entry = { isIntersecting: false };
  let flash = false;

  onMount(() => {
    observer = new IntersectionObserver(
      ([e]) => {
        entry = e;
      },
      {
        threshold: 0.69,
      }
    );
    observer.observe(sect);
  });

  onDestroy(() => {
    observer.disconnect();
  });
</script>

<section
  bind:this={sect}
  class="about bg-black observed relative overflow-hidden mobile:pointer-events-none"
>
  {#if entry.isIntersecting}
    <FluidSim />
  {/if}

  <div
    class="relative text-primary py-16 z-[1] mix-blend-difference pointer-events-none flex flex-col items-center justify-center text-center"
  >
    <div class="font-bold text-4xl mobile:text-3xl">Get to know me</div>
    <div class="font-black text-7xl py-8 mobile:text-4xl">
      I CREATE INTERACTIVE WEB DESIGN
    </div>
    <div
      class="py-12 font-semibold text-2xl p-page text-justify mobile:py-3 mobile:text-lg"
    >
      MBA (Finance & HR) student with SEBI Investor Certification and IIMB finance certification, client
servicing, documentation, and compliance fundamentals. Skilled in supporting advisory operations, preparing financial
data, managing records, and coordinating workflows with high accuracy. Seeking a Financial Advisory Support Analyst
role to assist advisors in delivering an exceptional client experience.
    </div>
  </div>

  <div
    class="absolute h-2/6 top-0 w-full pointer-events-none"
    style="background-image: linear-gradient(to bottom, rgb(var(--background)), transparent);"
  />
  <div
    class="absolute h-2/6 bottom-0 w-full pointer-events-none"
    style="background-image: linear-gradient(to top, rgb(var(--background)), transparent);"
  />
</section>
