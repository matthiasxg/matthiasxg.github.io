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

  let greenPathLength: number = 0;
  let bluePathLength: number = 0;
  let initialGreenDrawLength: number = 0;
  let initialBlueDrawLength: number = 0;

  const initialAnimationFraction = 0.25;
  const initialAnimationDuration = 2000;

  const getActualPoints = (points: { x: string; y: string }[]) => {
    const bodyRect = document.body.getBoundingClientRect();
    const totalWidth = bodyRect.width;
    const totalHeight = window.innerHeight;

    return points.map((point) => ({
      x: (parseFloat(point.x) / 100) * totalWidth,
      y: (parseFloat(point.y) / 100) * totalHeight,
    }));
  };

  const calculateAndSetPath = (
    path_points: { x: string; y: string }[],
    path_element: SVGPathElement,
  ): number => {
    const actualPoints = getActualPoints(path_points);
    if (actualPoints.length < 4 || !path_element) return 0;

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

    const length = path_element.getTotalLength();
    path_element.style.strokeDasharray = length.toString();
    path_element.style.strokeDashoffset = length.toString();
    return length;
  };

  const handleScroll = () => {
    if (
      !greenPathElement ||
      !bluePathElement ||
      !greenPathLength ||
      !bluePathLength
    )
      return;

    const scrollTop = window.scrollY || document.documentElement.scrollTop;
    const scrollHeight = document.documentElement.scrollHeight;
    const clientHeight = document.documentElement.clientHeight;
    const maxScrollTop = scrollHeight - clientHeight;
    const scrollFraction = maxScrollTop > 0 ? scrollTop / maxScrollTop : 0;
    const clampedScrollFraction = Math.min(1, Math.max(0, scrollFraction));

    const scrollDrivenGreenDraw = greenPathLength * clampedScrollFraction;
    const scrollDrivenBlueDraw = bluePathLength * clampedScrollFraction;

    const actualGreenDraw = Math.max(
      initialGreenDrawLength,
      scrollDrivenGreenDraw,
    );
    const actualBlueDraw = Math.max(
      initialBlueDrawLength,
      scrollDrivenBlueDraw,
    );

    requestAnimationFrame(() => {
      if (greenPathElement) {
        greenPathElement.style.strokeDashoffset = (
          greenPathLength - actualGreenDraw
        ).toString();
      }
      if (bluePathElement) {
        bluePathElement.style.strokeDashoffset = (
          bluePathLength - actualBlueDraw
        ).toString();
      }
    });
  };

  const handleResize = () => {
    greenPathLength = calculateAndSetPath(green_path_points, greenPathElement);
    bluePathLength = calculateAndSetPath(blue_path_points, bluePathElement);

    initialGreenDrawLength = greenPathLength * initialAnimationFraction;
    initialBlueDrawLength = bluePathLength * initialAnimationFraction;

    handleScroll();
  };

  onMount(() => {
    greenPathLength = calculateAndSetPath(green_path_points, greenPathElement);
    bluePathLength = calculateAndSetPath(blue_path_points, bluePathElement);

    initialGreenDrawLength = greenPathLength * initialAnimationFraction;
    initialBlueDrawLength = bluePathLength * initialAnimationFraction;

    const animationTargets = {
      greenOffset: greenPathLength,
      blueOffset: bluePathLength,
    };

    const initialAnimation = anime({
      targets: animationTargets,
      greenOffset: greenPathLength - initialGreenDrawLength,
      blueOffset: bluePathLength - initialBlueDrawLength,
      duration: initialAnimationDuration,
      easing: "easeInOutQuad",
      update: () => {
        if (greenPathElement) {
          greenPathElement.style.strokeDashoffset =
            animationTargets.greenOffset.toString();
        }
        if (bluePathElement) {
          bluePathElement.style.strokeDashoffset =
            animationTargets.blueOffset.toString();
        }
      },
    });

    window.addEventListener("scroll", handleScroll);
    window.addEventListener("resize", handleResize);

    return () => {
      window.removeEventListener("scroll", handleScroll);
      window.removeEventListener("resize", handleResize);
      anime.remove(animationTargets);
    };
  });
</script>

<svelte:window on:scroll={handleScroll} on:resize={handleResize} />

<svg
  bind:this={greenLine}
  class="absolute w-full h-[400vh] pointer-events-none"
  fill="none"
  stroke="var(--color-accent-green)"
  stroke-width="4"
>
  <path bind:this={greenPathElement} />
</svg>

<svg
  bind:this={blueLine}
  class="absolute w-full h-[400vh] pointer-events-none"
  fill="none"
  stroke="var(--color-accent-blue)"
  stroke-width="4"
>
  <path bind:this={bluePathElement} />
</svg>

<Landing />
<About />
<Projects />
<Contact />
