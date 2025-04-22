<script lang="ts">
    interface Project {
        title: string;
        description: string;
    }

    const projects: Project[] = [
        {
            title: "CPR Data Hub",
            description:
                "Centralizes and analyzes CPR data gathered by rescue teams. This tool parses device logs and visualizes key metrics on a web dashboard to enhance debriefing and skill development.",
        },
        {
            title: "More Projects Soon",
            description: "Check back later for updates.",
        },
    ];
</script>

<div
    class="w-screen min-h-screen flex flex-col items-center text-center text-regal-black bg-regal-white px-4 sm:px-8"
>
    <h1
        class="font-subheading text-5xl sm:text-6xl mb-8 mt-16 sm:mb-16 sm:mt-32"
    >
        Projects.
    </h1>
    <h1 class="font-subheading text-xl mb-8 sm:mb-16">
        Where I turned complex problems into elegant solutions.
    </h1>
    <div class="flex items-center justify-center w-full pb-16 sm:pb-32">
        <div
            class="grid grid-cols-1 sm:grid-cols-2 gap-8 sm:gap-16 w-full max-w-5xl px-4 sm:px-0"
        >
            {#each projects as project}
                <div
                    class="relative w-full sm:w-md aspect-square p-4 flex flex-col justify-between items-center"
                >
                    <span
                        class="absolute top-0 left-0 w-8 h-8 sm:w-16 sm:h-16 border-t-2 border-l-2 border-regal-black"
                    ></span>
                    <span
                        class="absolute top-0 right-0 w-8 h-8 sm:w-16 sm:h-16 border-t-2 border-r-2 border-regal-black"
                    ></span>
                    <span
                        class="absolute bottom-0 left-0 w-8 h-8 sm:w-16 sm:h-16 border-b-2 border-l-2 border-regal-black"
                    ></span>
                    <span
                        class="absolute bottom-0 right-0 w-8 h-8 sm:w-16 sm:h-16 border-b-2 border-r-2 border-regal-black"
                    ></span>

                    <h2
                        class="font-heading text-2xl sm:text-3xl text-regal-black mt-8 mb-4 sm:m-16 font-bold"
                    >
                        {project.title}
                    </h2>
                    <p
                        class="flex-grow text-base sm:text-xl text-regal-black px-4 pb-4 sm:p-8"
                    >
                        {project.description}
                    </p>
                </div>
            {/each}
        </div>
    </div>
</div>
