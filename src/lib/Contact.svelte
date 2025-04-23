<script lang="ts">
    import { onMount } from "svelte";
    import gsap from "gsap";
    import confetti from "canvas-confetti";

    let contactButton: HTMLElement;
    const email = "matthias.grill@icloud.com";
    onMount(() => {
        const enterAnimation = () => {
            gsap.to(contactButton, {
                scale: 1.05,
                duration: 0.3,
            });
        };

        const leaveAnimation = () => {
            gsap.to(contactButton, {
                scale: 1,
                duration: 0.3,
            });
        };

        const clickAnimation = (e: Event) => {
            e.preventDefault();

            gsap.to(contactButton, {
                scaleY: 0.9,
                scaleX: 1.1,
                duration: 0.1,
                yoyo: true,
                repeat: 1,
            });

            const rect = contactButton.getBoundingClientRect();
            const x = (rect.left + rect.right) / 2 / window.innerWidth;
            const y = (rect.top + rect.bottom) / 2 / window.innerHeight;

            confetti({
                particleCount: 100,
                spread: 70,
                origin: { x, y },
            });

            setTimeout(() => {
                window.location.href = `mailto:${email}`;
            }, 500);
        };

        contactButton.addEventListener("mouseenter", enterAnimation);
        contactButton.addEventListener("mouseleave", leaveAnimation);
        contactButton.addEventListener("click", clickAnimation);

        return () => {
            contactButton.removeEventListener("mouseenter", enterAnimation);
            contactButton.removeEventListener("mouseleave", leaveAnimation);
            contactButton.removeEventListener("click", clickAnimation);
        };
    });
</script>

<div
    class="w-screen min-h-screen flex flex-col justify-between text-center items-center bg-regal-white text-regal-black"
>
    <div class="flex flex-grow flex-col justify-center items-center">
        <h1 class="font-subheading text-5xl sm:text-6xl mb-8">
            Let's work <span
                class="text-accent-blue underline decoration-accent-green"
                >together</span
            >.
        </h1>
        <p
            bind:this={contactButton}
            class="ont-heading bg-regal-black text-regal-white px-4 py-2 font-bold text-xl sm:text-2xl rounded cursor-pointer"
        >
            Contact me
        </p>
    </div>

    <footer class="w-full py-8 bg-regal-white text-regal-black">
        <div
            class="container mx-auto flex flex-col sm:flex-row justify-between items-center gap-4"
        >
            <div class="text-xs text-center sm:text-left">
                © {new Date().getFullYear()} Matthias Grill. All rights reserved.
            </div>
            <div class="flex gap-4 text-xs">
                <a href="https://github.com/matthiasxg" class="hover:underline"
                    >GitHub</a
                >
                <a
                    href="https://www.linkedin.com/in/matthias-grill-83686a213/"
                    class="hover:underline">LinkedIn</a
                >
            </div>
        </div>
    </footer>
</div>
