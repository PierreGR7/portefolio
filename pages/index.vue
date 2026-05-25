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
  ogDescription: page.value?.description,
  twitterCard: 'summary',
  twitterTitle: page.value?.title,
  twitterDescription: page.value?.description,
})

const skills = [
  { label: 'Python', icon: 'i-simple-icons-python' },
  { label: 'SQL', icon: 'i-simple-icons-postgresql' },
  { label: 'Apache Spark', icon: 'i-simple-icons-apachespark' },
  { label: 'Airflow', icon: 'i-simple-icons-apacheairflow' },
  { label: 'Docker', icon: 'i-simple-icons-docker' },
  { label: 'Power BI', icon: 'i-simple-icons-powerbi' },
  { label: 'Azure', icon: 'i-simple-icons-microsoftazure' },
  { label: 'Git', icon: 'i-simple-icons-git' },
]
</script>

<template>
    <div class="mt-10 lg:mt-20 relative">
        <div class="mb-12">
            <h1 class="text-5xl font-bold tracking-tight mb-4">{{ page.informations.job }}</h1>
            <WorkAvailability :availability="page.informations.availability" :email="page.informations.email" />
        </div>

        <div class="max-w-3xl">
            <p class="mb-8 text-balance text-lg text-gray-600 dark:text-gray-400">
                {{ page.informations.description }}
            </p>
            <div class="flex flex-wrap gap-2 mb-8">
                <UBadge v-for="skill in skills" :key="skill.label" :label="skill.label" :icon="skill.icon"
                    color="gray" variant="soft" size="lg" class="font-medium" />
            </div>
            <div class="flex flex-wrap gap-4">
                <UButton icon="i-pepicons-pop-cv" :external="true" color="black" to="/pdf/CV_Pierre_Graef.pdf" size="xl"
                    label="My CV" target="_blank" />
                <UButton icon="i-material-symbols-mail" :external="true" :to="`mailto:${page.informations.email}`" size="xl"
                    label="Contact Me" />
            </div>
        </div>

        <UDivider class="py-14 lg:py-20" id="projects" />

        <div class="mb-10 md:mb-20">
            <h2 class="text-4xl font-bold mb-10">Projects</h2>
            <div class="flex flex-col md:grid md:grid-cols-2 gap-x-8 gap-y-16">
                <UBlogPost v-for="(project, index) in projects" :key="index" :title="project.title" :to="project._path"
                    :badge="project.badge" :description="project.description" :image="project.image"
                    :date="new Date(project.date).toLocaleDateString('fr-FR')" />
            </div>
        </div>
    </div>
</template>