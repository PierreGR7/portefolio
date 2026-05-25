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
                <UButton icon="i-material-symbols-mail" :to="`mailto:${page.informations.email}`" size="xl"
                    label="Contact Me" />
            </div>
        </div>

        <UDivider class="py-14 lg:py-20" />

        <div class="max-w-3xl mb-14 lg:mb-20">
            <h2 class="text-3xl font-bold mb-6">About Me</h2>
            <div class="space-y-4 text-gray-600 dark:text-gray-400 text-base leading-relaxed">
                <p>
                    I'm a <strong class="text-gray-900 dark:text-white">Data Engineer</strong> with international experience, passionate about transforming raw data into reliable, scalable systems. I specialize in designing end-to-end data architectures — from ingestion and transformation to dashboarding and delivery.
                </p>
                <p>
                    My technical toolkit covers the full data lifecycle: <strong class="text-gray-900 dark:text-white">Python</strong> for pipeline logic, <strong class="text-gray-900 dark:text-white">SQL</strong> for data modeling, <strong class="text-gray-900 dark:text-white">Spark & Airflow</strong> for distributed and orchestrated workflows, and <strong class="text-gray-900 dark:text-white">Power BI</strong> for business reporting. I'm also comfortable working with cloud environments, containers, and version-controlled infrastructure.
                </p>
                <p>
                    Outside of work, I build personal projects around topics I'm genuinely curious about — Formula 1 analytics, motorsport data, financial APIs — which you'll find in the projects section below.
                </p>
            </div>
        </div>

        <UDivider class="pb-14 lg:pb-20" id="projects" />

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