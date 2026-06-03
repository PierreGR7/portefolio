<script setup>

const { data: projects } = await useAsyncData('projets', () => queryContent('/projects')
    .where({ _extension: 'md' })
    .sort({ date: -1 })
    .find())

// Only list one card per article (hide alternate-language versions, e.g. *-fr).
const { data: articles } = await useAsyncData('articles', () => queryContent('/articles')
    .where({ _extension: 'md', lang: { $ne: 'fr' } })
    .sort({ date: -1 })
    .find())

const {data: page} = await useAsyncData('index', () => queryContent(`/`).findOne())

const shareImage = '/portfolio_clear.png'

useSeoMeta({
  title: page.value?.title,
  ogTitle: page.value?.title,
  description: page.value?.description,
  ogDescription: page.value?.description,
  ogImage: shareImage,
  twitterImage: shareImage,
  twitterCard: 'summary_large_image',
  twitterTitle: page.value?.title,
  twitterDescription: page.value?.description,
})

const toast = useToast()
const email = page.value?.informations?.email
const emailRevealed = ref(false)
const copied = ref(false)

async function revealEmail() {
  emailRevealed.value = true
  try {
    await navigator.clipboard.writeText(email)
    copied.value = true
    toast.add({
      title: 'Email copied!',
      description: email,
      icon: 'i-material-symbols-check-circle',
      color: 'green',
    })
    setTimeout(() => (copied.value = false), 2000)
  } catch (e) {
    // Clipboard unavailable (e.g. non-secure context): email stays visible to copy manually
  }
}

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
                <UButton v-if="!emailRevealed" icon="i-material-symbols-mail" size="xl"
                    label="Contact Me" @click="revealEmail" />
                <UButton v-else :icon="copied ? 'i-material-symbols-check-circle' : 'i-material-symbols-content-copy-outline'"
                    color="gray" variant="soft" size="xl" :label="copied ? 'Email copied!' : email"
                    @click="revealEmail" />
            </div>
        </div>

        <UDivider class="py-14 lg:py-20" id="projects" />

        <div class="mb-10 md:mb-20">
            <h2 class="text-4xl font-bold mb-3">Projects</h2>
            <p class="mb-10 text-gray-500 dark:text-gray-400">Hands-on data engineering builds</p>
            <div class="flex flex-col md:grid md:grid-cols-2 gap-x-8 gap-y-16">
                <UBlogPost v-for="(project, index) in projects" :key="index" :title="project.title" :to="project._path"
                    :badge="project.badge" :description="project.description" :image="project.image"
                    :date="new Date(project.date).toLocaleDateString('fr-FR')" />
            </div>
        </div>

        <template v-if="articles?.length">
            <UDivider class="py-14 lg:py-20" id="articles" />

            <div class="mb-10 md:mb-20">
                <h2 class="text-4xl font-bold mb-3">Articles</h2>
                <p class="mb-10 text-gray-500 dark:text-gray-400">Opinions, takes and news on data, AI and tech</p>
                <div class="flex flex-col md:grid md:grid-cols-2 gap-x-8 gap-y-16">
                    <UBlogPost v-for="(article, index) in articles" :key="index" :title="article.title" :to="article._path"
                        :badge="article.badge" :description="article.description" :image="article.image"
                        :date="new Date(article.date).toLocaleDateString('fr-FR')" />
                </div>
            </div>
        </template>
    </div>
</template>