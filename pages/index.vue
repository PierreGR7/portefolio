<script setup>

const { data: projects } = await useAsyncData('projets', () => queryContent('/projects')
    .where({ _extension: 'md' })
    .sort({ date: -1 })
    .find())

</script>

<template>
    <div class="mt-10 lg:mt-20 relative">
        <div class="mb-12 ">
            <h1 class="text-5xl font-bold tracking-tight mb-4">Data Scientist</h1>
            <WorkAvailability :availability="true" />
        </div>

        <div class="max-w-3xl">

            <p class="text-2xl font-bold mb-4">Python | R | SQL | Power BI | Excel</p>

            <p class="mb-8 text-balance text-lg">I am a results-oriented data analyst, skilled in advanced analytics and
                proficient in programming
                languages (like Python, R, SQL) and other analytics toolkits (like Power BI, Excel). With a proven
                track
                record of translating raw data into actionable insights, I consistently deliver impactful outcomes.
                My
                proven ability to transform complex datasets into valuable information has played a pivotal role in
                facilitating informed decision-making processes.
            </p>
            <div class="flex gap-4">
                <UButton icon="i-pepicons-pop-cv" :external="true" color="black" to="/pdf/CV_Pierre_Graef.pdf" size="xl" label="My CV" />

                <UButton icon="i-material-symbols-mail" to="mailto:pierre.graef@gmail.com" size="xl" label="Contact Me" />

            </div>

        </div>
        <UDivider class="py-20" id="projects"/>
        <div class="mb-10 md:mb-20">
            <h2 class="text-4xl font-bold mb-10">Projects</h2>
            <div class="grid md:grid-cols-2 gap-10">
                <UBlogPost v-for="(project, index) in projects" :key="index" :title="project.title" :to="project._path" :badge="project.badge"
                    :description="project.description" :image="project.image" :date="new Date(project.date).toLocaleDateString('fr-FR')"/>
            </div>
        </div>
    </div>
</template>