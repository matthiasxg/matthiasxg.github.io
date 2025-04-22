<script lang="ts">
  import { onMount } from "svelte";
  import { gsap } from "gsap";
  import { ScrollTrigger } from "gsap/ScrollTrigger";

  import About from "./lib/About.svelte";
  import Contact from "./lib/Contact.svelte";
  import Landing from "./lib/Landing.svelte";
  import Projects from "./lib/Projects.svelte";

  gsap.registerPlugin(ScrollTrigger);

  onMount(() => {
    // Animate the green spline
    gsap.to("#green-spline", {
      scrollTrigger: {
        trigger: "#green-spline",
        start: "top bottom",
        end: "bottom top",
        scrub: true,
      },
      strokeDashoffset: 0,
    });

    // Animate the blue spline
    gsap.to("#blue-spline", {
      scrollTrigger: {
        trigger: "#blue-spline",
        start: "top bottom",
        end: "bottom top",
        scrub: true,
      },
      strokeDashoffset: 0,
    });
  });
</script>

<!-- <svg class="absolute inset-0 w-full h-full pointer-events-none">
  <path
    id="green-spline"
    d="M100,0 C150,200 50,400 100,600"
    class="fill-none stroke-green-500 stroke-[4px] [stroke-dasharray:1000] [stroke-dashoffset:1000]"
  />
  <path
    id="blue-spline"
    d="M300,0 C350,200 250,400 300,600"
    class="fill-none stroke-blue-500 stroke-[4px] [stroke-dasharray:1000] [stroke-dashoffset:1000]"
  />
</svg> -->

<Landing />
<About />
<Projects />
<Contact />
