<script lang="ts">
  import { onMount } from "svelte";
  import anime from "animejs";
  import About from "./lib/About.svelte";
  import Contact from "./lib/Contact.svelte";
  import Landing from "./lib/Landing.svelte";
  import Projects from "./lib/Projects.svelte";

  const green_path_points = [
    { x: "0%", y: "80%" },
    { x: "70%", y: "110%" },
    { x: "-20%", y: "160%" },

    { x: "20%", y: "250%" },
    { x: "-20%", y: "300%" },

    { x: "0%", y: "370%" },
    { x: "50%", y: "360%" },
    { x: "50%", y: "360%" },
  ];

  const blue_path_points = [
    { x: "80%", y: "0%" },
    { x: "50%", y: "50%" },
    { x: "70%", y: "100%" },

    { x: "100%", y: "150%" },
    { x: "100%", y: "180%" },

    { x: "80%", y: "220%" },
    { x: "100%", y: "300%" },

    { x: "100%", y: "340%" },
    { x: "50%", y: "360%" },
    { x: "50%", y: "360%" },
  ];

  let greenLine: SVGSVGElement;
  let greenPathElement: SVGPathElement;
  let blueLine: SVGSVGElement;
  let bluePathElement: SVGPathElement;

  onMount(() => {
    const getActualPoints = (points: { x: string; y: string }[]) => {
      const bodyRect = document.body.getBoundingClientRect();
      const totalWidth = bodyRect.width;

      return points.map((point) => ({
        x: (parseFloat(point.x) / 100) * totalWidth,
        y: (parseFloat(point.y) / 100) * window.innerHeight,
      }));
    };

    const updatePath = (
      path_points: { x: string; y: string }[],
      path_element: SVGPathElement,
    ) => {
      const actualPoints = getActualPoints(path_points);
      if (actualPoints.length < 4) return;

      let pathData = `M ${actualPoints[0].x},${actualPoints[0].y}`;

      for (let i = 1; i < actualPoints.length - 2; i++) {
        const p0 = actualPoints[i - 1];
        const p1 = actualPoints[i];
        const p2 = actualPoints[i + 1];
        const p3 = actualPoints[i + 2];

        const t0 = 0.5;
        const t1 = 0.5;

        const m1x = t1 * (p2.x - p0.x);
        const m1y = t1 * (p2.y - p0.y);
        const m2x = t0 * (p3.x - p1.x);
        const m2y = t0 * (p3.y - p1.y);

        const c1x = p1.x + m1x / 3;
        const c1y = p1.y + m1y / 3;
        const c2x = p2.x - m2x / 3;
        const c2y = p2.y - m2y / 3;

        pathData += ` C ${c1x},${c1y} ${c2x},${c2y} ${p2.x},${p2.y}`;
      }

      path_element.setAttribute("d", pathData);

      anime({
        targets: path_element,
        strokeDashoffset: [anime.setDashoffset, 0],
        easing: "easeInOutSine",
        duration: 2000,
        loop: false,
      });
    };

    updatePath(green_path_points, greenPathElement);
    updatePath(blue_path_points, bluePathElement);
  });
</script>

<svg
  bind:this={greenLine}
  class="absolute w-full h-[400vh] pointer-events-none"
  fill="none"
  stroke="var(--color-accent-green)"
  stroke-width="4"
>
  <path bind:this={greenPathElement} id="page-spanning-path" />
</svg>

<svg
  bind:this={blueLine}
  class="absolute w-full h-[400vh] pointer-events-none"
  fill="none"
  stroke="var(--color-accent-blue)"
  stroke-width="4"
>
  <path bind:this={bluePathElement} id="page-spanning-path" />
</svg>

<Landing />
<About />
<Projects />
<Contact />
