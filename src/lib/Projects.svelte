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
    class="w-full min-h-screen flex flex-col items-center text-center text-regal-black bg-regal-white px-4 sm:px-8"
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
        <div class="w-full md:max-w-5xl mx-auto px-4">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8 sm:gap-12">
                {#each projects as project}
                    <div class="aspect-square w-full">
                        <div class="relative w-full h-full p-4 flex flex-col">
                            <span
                                class="absolute top-0 left-0 w-8 h-8 sm:w-12 sm:h-12 border-t-2 border-l-2 border-regal-black"
                            ></span>
                            <span
                                class="absolute top-0 right-0 w-8 h-8 sm:w-12 sm:h-12 border-t-2 border-r-2 border-regal-black"
                            ></span>
                            <span
                                class="absolute bottom-0 left-0 w-8 h-8 sm:w-12 sm:h-12 border-b-2 border-l-2 border-regal-black"
                            ></span>
                            <span
                                class="absolute bottom-0 right-0 w-8 h-8 sm:w-12 sm:h-12 border-b-2 border-r-2 border-regal-black"
                            ></span>

                            <h2
                                class="font-heading text-3xl text-regal-black font-bold lg:m-20 md:m-5 m-5 text-center"
                            >
                                {project.title}
                            </h2>

                            <div
                                class="flex-grow flex md:items-start items-center justify-center"
                            >
                                <p
                                    class="text-base sm:text-xl text-regal-black px-4 text-center"
                                >
                                    {project.description}
                                </p>
                            </div>
                        </div>
                    </div>
                {/each}
            </div>
        </div>
    </div>
</div>
