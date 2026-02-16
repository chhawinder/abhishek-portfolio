<script>
  import { onMount } from "svelte";
  import Spacer from "./Spacer.svelte";
  import gsap from "gsap";
  import ScrollTrigger from "gsap/ScrollTrigger";

  gsap.registerPlugin(ScrollTrigger);

  let theme, skillsContainer;
  const defaultTheme = "#455A64";
  $: theme = defaultTheme;
  $: skillShadow = "#222";

  async function getAverageColor(src) {
    return new Promise((resolve) => {
      let context = document.createElement("canvas").getContext("2d");
      context.imageSmoothingEnabled = true;
      let img = new Image();
      img.src = src;
      img.crossOrigin = "";
      img.onload = () => {
        context.drawImage(img, 0, 0, img.width, img.height);
        let imageData = context.getImageData(0, 0, img.width, img.height);
        let data = imageData.data;
        let nonTransparentPixels = 0;
        let r = 0;
        let g = 0;
        let b = 0;

        for (let i = 0; i < data.length; i += 4) {
          if (data[i + 3] != 0) {
            r += data[i];
            g += data[i + 1];
            b += data[i + 2];
            nonTransparentPixels++;
          }
        }
        r = Math.floor(r / nonTransparentPixels);
        g = Math.floor(g / nonTransparentPixels);
        b = Math.floor(b / nonTransparentPixels);
        let hex = rgbToHex(r, g, b);
        resolve(hex);
      };
    });
  }

  function getVisibleColor(color) {
    let r = parseInt(color.substring(1, 3), 16);
    let g = parseInt(color.substring(3, 5), 16);
    let b = parseInt(color.substring(5, 7), 16);

    let brightness = (r * 299 + g * 587 + b * 114) / 1000;

    if (brightness < 128) {
      return "#FFFFFF";
    } else {
      return "#000000";
    }
  }

  function componentToHex(c) {
    var hex = c.toString(16);
    return hex.length == 1 ? "0" + hex : hex;
  }

  function rgbToHex(r, g, b) {
    return "#" + componentToHex(r) + componentToHex(g) + componentToHex(b);
  }

  const skills = [
    { name: "c", imageUrl: "/skills/c.webp", color: "#626bb9" },
    { name: "c++", imageUrl: "/skills/c++.webp", color: "#659ad1" },
    { name: "python", imageUrl: "/skills/python.webp", color: "#ffde56" },
    
    { name: "html", imageUrl: "/skills/html.webp", color: "#fe7816" },
    { name: "css", imageUrl: "/skills/css.webp", color: "#2a65f0" },
    { name: "js", imageUrl: "/skills/js.webp", color: "#ffdf00" },
    
   
    { name: "bootstrap", imageUrl: "/skills/bootstrap.webp", color: "#8b13fd" },
    
    { name: "tailwind", imageUrl: "/skills/tailwind.webp", color: "#00c0cd" },
    { name: "svelte", imageUrl: "/skills/svelte.webp", color: "#ff2a29" },
    { name: "react", imageUrl: "/skills/react.webp", color: "#05bfd5" },
    { name: "nodejs", imageUrl: "/skills/nodejs.webp", color: "#8cc041" },
    { name: "flask", imageUrl: "/skills/flask.webp", color: "#797979" },
    { name: "express", imageUrl: "/skills/express.webp", color: "#494949" },
    { name: "next", imageUrl: "/skills/next.webp", color: "#222222" },
    
    { name: "fastapi", imageUrl: "/skills/fastapi.webp", color: "#05998b" },
    { name: "postman", imageUrl: "/skills/postman.webp", color: "#ee6d3f" },
     { name: "bash", imageUrl: "/skills/bash.webp", color: "#bebebe" },
    
    { name: "git", imageUrl: "/skills/git.webp", color: "#ef5132" },
    { name: "github", imageUrl: "/skills/github.webp", color: "#000000" },
    {
      name: "githubActions",
      imageUrl: "/skills/githubActions.webp",
      color: "#79b8ff",
    },
    { name: "gitlab", imageUrl: "/skills/gitlab.webp", color: "#fc6d24" },
    { name: "netlify", imageUrl: "/skills/netlify.webp", color: "#40b5bb" },
    { name: "replit", imageUrl: "/skills/replit.webp", color: "#f26301" },
    { name: "heroku", imageUrl: "/skills/heroku.webp", color: "#7d57c1" },
    { name: "vercel", imageUrl: "/skills/vercel.webp", color: "#2a2a2a" },
    
    { name: "figma", imageUrl: "/skills/figma.webp", color: "#a25aff" },
  ];
</script>

<div class="h-16 bg-background" />
<Spacer flip startColor={theme} endColor="#111826" />
<section
  class="skills p-page z-[2]"
  style="background-color: {theme}; transition:800ms;"
>
  <h1
    class="transition-300 text-3xl font-semibold text-center"
    style="color:{getVisibleColor(theme)}"
  >
    Tech I've worked with
  </h1>
  <div
    bind:this={skillsContainer}
    class="skills-container flex flex-row justify-evenly flex-wrap py-8 gap-x-10 gap-y-10"
  >
    {#each skills as skill}
      <img
        class="skill-icon w-14 aspect-square object-contain mobile:w-8"
        style="--skill-shadow:{getVisibleColor(theme)};"
        src={skill.imageUrl}
        alt={skill.name}
        data-blobity-tooltip={skill.name}
        on:mouseover={async () => {
          theme =
            skill.color && skill.color != "#"
              ? skill.color
              : await getAverageColor(skill.imageUrl);
        }}
        on:focus={() => {}}
        on:mouseleave={() => {
          theme = defaultTheme;
        }}
        draggable="false"
      />
    {/each}
  </div>
</section>
<Spacer startColor={theme} endColor="#111826" />

<style>
  .skill-icon {
    transition: scale 300ms, filter 300ms;
  }

  @keyframes skill-hover-anim {
    from {
      rotate: 0deg;
    }
    to {
      rotate: 359.9deg;
    }
  }

  .skills-container:has(.skill-icon:hover) .skill-icon {
    opacity: 0.5;
    scale: 0.8;
  }

  .skills-container:hover .skill-icon:hover {
    opacity: 1;
    animation: skill-hover-anim ease-out 200ms;
    scale: 1.5;
    filter: drop-shadow(3px 3px 6px var(--skill-shadow));
  }
</style>
