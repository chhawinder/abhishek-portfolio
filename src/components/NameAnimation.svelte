<script>
  import { fly } from 'svelte/transition'
  import { backOut } from 'svelte/easing'

  const lines = [
    'Welcome',
    '',
    'To',
    '',
    "chhawinder's",
    'Portfolio🪄'
  ]

  let animate = true; // Automatically start the animation
</script>

<div class="lines">
  {#each lines as line, i}
    <div class="overflow">
      <span
        class="line"
        in:fly={{
          y: 100,
          delay: 300 * i,
          easing: backOut
        }}
        out:fly={{
          y: -100,
          delay: 300 * (lines.length - i - 1),
          easing: backOut
        }}
      >
        {line}
      </span>
    </div>
  {/each}
</div>

<style>
  .lines {
    max-width: 800px;
    font-size: 3rem;
    font-weight: 900;
    text-align: center;
  }

  .overflow {
    display: inline-block;
    overflow: hidden;
    vertical-align: bottom;
  }

  .line {
    display: inline-block;
    margin: 0.2rem;
    text-transform: uppercase;
    text-shadow: 2px 0 10px hsla(0, 0%, 0%, 0.2);
    color: white; /* Set the text color to white */
    transition: background-color 0.3s ease, transform 0.3s ease; /* Add transform transition */
    padding: 0.2rem 0.5rem;
    border-radius: 0.5rem;
  }

  .line:hover {
    background-color: rgba(255, 255, 255, 0.1);
    transform: scale(1.05);
  }
</style>
