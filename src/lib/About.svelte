<script lang="ts">
    interface Education {
        title: string;
        institution: string;
        period: string;
        align: "left" | "right";
    }

    const educationItems: Education[] = [
        {
            title: "Technical High School Diploma in Computer Science",
            institution: "HTL Wiener Neustadt",
            period: "2014 - 2019",
            align: "left",
        },
        {
            title: "Civil Service",
            institution: "Red Cross",
            period: "2019 - 2020",
            align: "right",
        },
        {
            title: "Bachelor's Programme Computer Science",
            institution: "Technical University Graz",
            period: "2020 - 2024",
            align: "left",
        },
        {
            title: "Student Assistant",
            institution: "Technical University Graz",
            period: "2022 - 2024",
            align: "right",
        },
        {
            title: "Medical Data Systems Developer",
            institution: "Medical University Graz",
            period: "2024 - present",
            align: "left",
        },
        {
            title: "Master's Programme Computer Science",
            institution: "Technical University Graz",
            period: "2024 - present",
            align: "right",
        },
    ];
</script>

<div
    class="w-screen min-h-screen flex flex-col items-center text-center text-regal-black bg-regal-white px-4 sm:px-8"
>
    <h1 class="font-subheading text-5xl sm:text-6xl mb-16 mt-32">
        Experience.
    </h1>
    <h1 class="font-subheading text-xl mb-32 sm:mb-16">
        Where I've applied and developed my skills.
    </h1>
    <div class="flex-grow flex items-center justify-center w-full">
        <div
            class="grid grid-cols-[1fr_auto_1fr] text-regal-black font-heading text-base sm:text-xl w-full max-w-4xl"
        >
            <div></div>
            <div class="h-16 flex justify-center items-center relative">
                <div class="w-0.5 bg-regal-black h-full absolute"></div>
            </div>
            <div></div>
            {#each educationItems as item}
                {#if item.align === "left"}
                    <div
                        class="h-full flex flex-col justify-center text-right pr-8 sm:pr-16"
                    >
                        <h1 class="">{item.title}</h1>
                        <p class="text-xs sm:text-sm">{item.institution}</p>
                        <p class="text-xs sm:text-sm">{item.period}</p>
                    </div>
                    <div
                        class="h-full flex justify-center items-center relative"
                    >
                        <div class="w-0.5 bg-regal-black h-full absolute"></div>
                        <div
                            class="w-2 sm:w-4 h-0.5 bg-regal-black absolute right-0"
                        ></div>
                    </div>
                    <div></div>
                {:else}
                    <div></div>
                    <div
                        class="h-full flex justify-center items-center relative"
                    >
                        <div class="w-0.5 bg-regal-black h-full absolute"></div>
                        <div
                            class="w-2 sm:w-4 h-0.5 bg-regal-black absolute left-0"
                        ></div>
                    </div>
                    <div
                        class="h-full flex flex-col justify-center text-left pl-8 sm:pl-16"
                    >
                        <h1>{item.title}</h1>
                        <p class="text-xs sm:text-sm">{item.institution}</p>
                        <p class="text-xs sm:text-sm">{item.period}</p>
                    </div>
                {/if}
            {/each}
            <div></div>
            <div class="h-16 flex justify-center items-center relative">
                <div class="w-0.5 bg-regal-black h-full absolute"></div>
            </div>
            <div></div>
        </div>
    </div>
</div>
