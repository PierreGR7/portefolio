<script setup>

const {data: page} = await useAsyncData('index', () => queryContent(`/`).findOne())

</script>

<template>
    <div class="overflow-x-hidden">
        <div
            class="lg:w-[33%] lg:min-h-screen bg-gray-50 dark:bg-[#04061d] lg:fixed max-lg:border-b lg:border-r dark:border-gray-800 flex justify-center lg:justify-end p-12 relative">
            <div
                class="absolute inset-0 landing-grid z-[0] [mask-image:radial-gradient(100%_100%_at_top_left,white,transparent)]" />
            <UColorModeToggle class="absolute top-4 max-lg:right-4 lg:left-4" />
            <div class="max-w-sm mt-6 lg:mt-28 z-[1]">
                <NuxtLink to="/">
                    <img class="w-40 h-40 object-cover rounded-full max-lg:mx-auto lg:ml-auto mb-8"
                    :src="page.informations.profile.url"
                        :alt="page.informations.name">
                </NuxtLink>
                <div>
                    <h2 class=" text-4xl font-bold text-nowrap">{{ page.informations.name }}</h2>
                    <p class="text-center lg:text-right text-lg hover:text-primary transition">
                        <a :href="'mailto:' + page.informations.email"
                            >{{page.informations.email}}</a></p>
                    <div class="mt-6 lg:mt-10 flex justify-center lg:justify-end">
                        <UButton v-if="page.informations.link.x" icon="i-simple-icons-x" size="xl" color="gray" variant="ghost"
                            :to="page.informations.link.x" target="_blank" />
                        <UButton v-if="page.informations.link.linkedin" icon="i-simple-icons-linkedin" size="xl" color="gray" variant="ghost"
                            :to="page.informations.link.linkedin" target="_blank" />
                        <UButton v-if="page.informations.link.instagram" icon="i-simple-icons-instagram" size="xl" color="gray" variant="ghost"
                            :to="page.informations.link.instagram" target="_blank" />
                        <UButton v-if="page.informations.link.github" icon="i-simple-icons-github" size="xl" color="gray" variant="ghost"
                            :to="page.informations.link.github" target="_blank" />
                        <UButton v-if="page.informations.link.kaggle" icon="cib:kaggle" size="xl" color="gray" variant="ghost"
                            :to="page.informations.link.kaggle" target="_blank" />
                    </div>
                </div>

                <div v-if="page.informations.experience?.length" class="mt-8 lg:mt-10 w-full">
                    <p class="text-xs font-semibold uppercase tracking-widest text-gray-400 dark:text-gray-500 mb-4 text-center lg:text-right">Experience</p>
                    <ul class="space-y-3">
                        <li v-for="(exp, i) in page.informations.experience" :key="i"
                            class="text-center lg:text-right rounded-lg px-3 py-2 bg-white/50 dark:bg-white/5 border border-gray-200/70 dark:border-gray-800 transition hover:border-primary/50">
                            <p class="font-semibold text-sm leading-tight">{{ exp.role }}</p>
                            <p class="text-sm text-primary font-medium">{{ exp.company }}</p>
                            <p class="text-xs text-gray-400 dark:text-gray-500 mt-1">{{ exp.period }}</p>
                            <p class="text-xs text-gray-400 dark:text-gray-500">{{ exp.location }}</p>
                        </li>
                    </ul>
                </div>

                <div class="max-lg:hidden absolute bottom-6 right-1/2 translate-x-1/2">
                    <p> Copyright © {{ new Date().getFullYear() }}
                    </p>
                </div>
            </div>
        </div>

        <div class="lg:ml-[33%] lg:w-[67%] p-4 lg:p-8">
            <div class="lg:max-w-5xl mx-auto">
                <slot />
            </div>
            <div class="text-center lg:hidden">
                <p> Copyright © {{ new Date().getFullYear() }}
                </p>
            </div>
        </div>
        <script src="https://cdn.botpress.cloud/webchat/v2.2/inject.js"></script>
        <script src="https://files.bpcontent.cloud/2024/10/25/07/20241025073130-UF7G5WKE.js"></script>
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