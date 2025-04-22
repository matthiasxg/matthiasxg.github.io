<script lang="ts">
  import { onMount } from "svelte";
  import { gsap } from "gsap";
  import { ScrollTrigger } from "gsap/ScrollTrigger";

  import About from "./lib/About.svelte";
  import Contact from "./lib/Contact.svelte";
  import Landing from "./lib/Landing.svelte";
  import Projects from "./lib/Projects.svelte";

  gsap.registerPlugin(ScrollTrigger);

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
    { x: "70%", y: "0%" },
    { x: "30%", y: "50%" },
    { x: "70%", y: "100%" },
    { x: "100%", y: "150%" },
    { x: "100%", y: "180%" },
    { x: "80%", y: "220%" },
    { x: "100%", y: "300%" },
    { x: "100%", y: "340%" },
    { x: "50%", y: "360%" },
    { x: "50%", y: "360%" },
  ];

  let greenPathElement: SVGPathElement;
  let bluePathElement: SVGPathElement;

  const getActualPoints = (points: { x: string; y: string }[]) => {
    return points.map((point) => ({
      x: (parseFloat(point.x) / 100) * window.innerWidth,
      y: (parseFloat(point.y) / 100) * window.innerHeight,
    }));
  };

  const calculateAndSetPath = (
    path_points: { x: string; y: string }[],
    path_element: SVGPathElement,
  ) => {
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
  };

  onMount(() => {
    if (!greenPathElement || !bluePathElement) return;

    calculateAndSetPath(green_path_points, greenPathElement);
    calculateAndSetPath(blue_path_points, bluePathElement);

    const greenPathLength = greenPathElement.getTotalLength();
    const bluePathLength = bluePathElement.getTotalLength();

    const greenInitialTarget = greenPathLength * (1 - 0.05);
    const blueInitialTarget = bluePathLength * (1 - 0.1);

    const initialTl = gsap.timeline({
      onComplete: setupScrollAnimations,
    });

    initialTl.to(greenPathElement, {
      strokeDashoffset: greenInitialTarget,
      duration: 1,
    });

    initialTl.to(
      bluePathElement,
      {
        strokeDashoffset: blueInitialTarget,
        duration: 1,
      },
      "<",
    );

    function setupScrollAnimations() {
      gsap.fromTo(
        greenPathElement,
        { ease: "linear", strokeDashoffset: greenInitialTarget },
        {
          ease: "linear",
          strokeDashoffset: 0,
          scrollTrigger: {
            trigger: document.body,
            start: "top",
            end: "bottom bottom",
            scrub: 1,
          },
        },
      );

      gsap.fromTo(
        bluePathElement,
        { ease: "linear", strokeDashoffset: blueInitialTarget },
        {
          ease: "linear",
          strokeDashoffset: 0,
          scrollTrigger: {
            trigger: document.body,
            start: "top",
            end: "bottom bottom",
            scrub: 1,
          },
        },
      );
    }

    const handleResize = () => {
      calculateAndSetPath(green_path_points, greenPathElement);
      calculateAndSetPath(blue_path_points, bluePathElement);
      ScrollTrigger.refresh();
    };

    window.addEventListener("resize", handleResize);

    return () => {
      window.removeEventListener("resize", handleResize);
      ScrollTrigger.getAll().forEach((trigger) => trigger.kill());
    };
  });
</script>

<svg class="absolute top-0 left-0 w-full h-[400vh] pointer-events-none">
  <path
    bind:this={greenPathElement}
    id="green-spline"
    class="fill-none stroke-accent-green stroke-[4px]"
  />
  <path
    bind:this={bluePathElement}
    id="blue-spline"
    class="fill-none stroke-accent-blue stroke-[4px]"
  />
</svg>

<Landing />
<About />
<Projects />
<Contact />
