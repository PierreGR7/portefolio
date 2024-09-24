<template>
    <div class="overflow-x-hidden">
        <div
            class="lg:w-[33%] lg:min-h-screen bg-gray-50 dark:bg-[#04061d] lg:fixed max-lg:border-b lg:border-r dark:border-gray-800 flex justify-center lg:justify-end p-12 relative">
            <div
                class="absolute inset-0 landing-grid z-[-1] [mask-image:radial-gradient(100%_100%_at_top_left,white,transparent)]" />
            <UColorModeToggle class="absolute top-4 max-lg:right-4 lg:left-4" />
            <div class="max-w-sm mt-10 lg:mt-28">
                <img class="w-40 rounded-full max-lg:mx-auto lg:ml-auto mb-8"
                    src="https://media.licdn.com/dms/image/v2/D4E03AQF53CxzwgSGZA/profile-displayphoto-shrink_800_800/profile-displayphoto-shrink_800_800/0/1672141377864?e=1732752000&v=beta&t=Har4AHtan0n3-iRznV47ohhoi5MGUIAU749Bx6DpGaw" alt="">
                <div>
                    <h2 class=" text-4xl font-bold text-nowrap">Pierre Graef</h2>
                    <p class="text-center lg:text-right text-lg"><a href="mailto:pierre.graef@gmail.com">pierre.graef@gmail.com</a></p>
                    <div class="mt-10 flex justify-center lg:justify-end">
                        <UButton icon="i-simple-icons-x" size="xl" color="gray" variant="ghost"
                            to="https://x.com/nicohrte/" target="_blank" />
                        <UButton icon="i-simple-icons-linkedin" size="xl" color="gray" variant="ghost"
                            to="https://www.linkedin.com/in/nicolas-harter-b34913207/" target="_blank" />
                        <UButton icon="i-simple-icons-instagram" size="xl" color="gray" variant="ghost"
                            to="https://github.com/NicolasHrt" target="_blank" />
                        <UButton icon="i-simple-icons-github" size="xl" color="gray" variant="ghost"
                            to="https://github.com/NicolasHrt" target="_blank" />

                    </div>
                </div>
                <div class="max-lg:hidden absolute bottom-6 right-1/2 translate-x-1/2">
                    <p> Copyright © {{ new Date().getFullYear() }}
                    </p>
                </div>
            </div>
        </div>

        <div class="lg:ml-[33%] lg:w-[67%] p-8">
            <div class="lg:max-w-5xl mx-auto">
                <slot />
            </div>
        </div>
    </div>
</template>


<style scoped>
.landing-grid {
  background-size: 100px 100px;
  background-image: linear-gradient(to right, rgb(var(--color-gray-200)) 1px, transparent 1px),
  linear-gradient(to bottom, rgb(var(--color-gray-200)) 1px, transparent 1px);
}

.dark {
  .landing-grid {
    background-image: linear-gradient(to right, rgb(var(--color-gray-800)) 1px, transparent 1px),
    linear-gradient(to bottom, rgb(var(--color-gray-800)) 1px, transparent 1px);
  }
}
</style>