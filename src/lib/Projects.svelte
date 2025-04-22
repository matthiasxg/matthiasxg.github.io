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
    class="w-screen h-screen flex flex-col items-center text-center text-regal-black bg-regal-white"
>
    <h1 class="font-subheading text-6xl mb-32 mt-32">Projects.</h1>
    <div class="flex items-center justify-center w-full">
        <div class="grid grid-cols-2 gap-16 m-32">
            {#each projects as project}
                <div
                    class="relative w-md aspect-square p-4 flex flex-col justify-between items-center"
                >
                    <span
                        class="absolute top-0 left-0 w-16 h-16 border-t-2 border-l-2 border-regal-black"
                    ></span>
                    <span
                        class="absolute top-0 right-0 w-16 h-16 border-t-2 border-r-2 border-regal-black"
                    ></span>
                    <span
                        class="absolute bottom-0 left-0 w-16 h-16 border-b-2 border-l-2 border-regal-black"
                    ></span>
                    <span
                        class="absolute bottom-0 right-0 w-16 h-16 border-b-2 border-r-2 border-regal-black"
                    ></span>

                    <h2
                        class="font-heading text-3xl text-regal-black m-16 font-bold"
                    >
                        {project.title}
                    </h2>
                    <p class="flex-grow text-xl text-regal-black p-8">
                        {project.description}
                    </p>
                </div>
            {/each}
        </div>
    </div>
</div>
