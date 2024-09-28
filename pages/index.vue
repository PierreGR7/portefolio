<script setup>

const { data: projects } = await useAsyncData('projets', () => queryContent('/projects')
    .where({ _extension: 'md' })
    .sort({ date: -1 })
    .find())

const {data: page} = await useAsyncData('index', () => queryContent(`/`).findOne())

useSeoMeta({
  title: page.value?.title,
  ogTitle: page.value?.title,
  description: page.value?.description,
  ogDescription: page.value?.description
})
</script>

<template>
    <div class="mt-10 lg:mt-20 relative">
        <div class="mb-12 ">
            <h1 class="text-5xl font-bold tracking-tight mb-4">{{ page.informations.job }}</h1>
            <WorkAvailability :availability="page.informations.availability" />
        </div>

        <div class="max-w-3xl">

            <p class="text-2xl font-bold mb-4">{{page.informations.skills}}</p>

            <p class="mb-8 text-balance text-lg">
                {{ page.informations.description }}
            </p>
            <div class="flex gap-4">
                <UButton icon="i-pepicons-pop-cv" :external="true" color="black" to="/pdf/CV_Pierre_Graef.pdf" size="xl"
                    label="My CV" />

                <UButton icon="i-material-symbols-mail" :to="`mailto:${page.informations.email}`" size="xl"
                    label="Contact Me" />

            </div>

        </div>
        <UDivider class="py-14 lg:py-20" id="projects" />
        <div class="mb-10 md:mb-20">
            <h2 class="text-4xl font-bold mb-10">Projects</h2>
            <div class="grid md:grid-cols-2 gap-10">
                <UBlogPost v-for="(project, index) in projects" :key="index" :title="project.title" :to="project._path"
                    :badge="project.badge" :description="project.description" :image="project.image"
                    :date="new Date(project.date).toLocaleDateString('fr-FR')" />
            </div>
        </div>
    </div>
</template>