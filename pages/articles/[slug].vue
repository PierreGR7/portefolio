<script setup>
const route = useRoute()
import { withoutTrailingSlash } from 'ufo'


onMounted(() => {
  MathJax.typeset();
});


const { data: page } = await useAsyncData(route.path, () => queryContent(route.path).findOne())

useSeoMeta({
  title: page.value?.title ? `${page.value.title} — Pierre Graef` : 'Pierre Graef',
  ogTitle: page.value?.title,
  description: page.value?.description,
  ogDescription: page.value?.description,
  twitterCard: 'summary_large_image',
  twitterTitle: page.value?.title,
  twitterDescription: page.value?.description,
  ogImage: page.value?.image?.src ?? undefined,
})

const links = [{
  label: 'Home',
  icon: 'i-heroicons-home',
  to: '/'
}, {
  label: 'Articles',
  to: '/#articles',
  icon: 'material-symbols:article'
}, {
  label: page.value ? page.value.title : 'Random',
  icon: "material-symbols:article"
}]


const headline = computed(() => findPageHeadline(page.value))

const { data: surround } = await useAsyncData(`${route.path}-surround`, () => queryContent('/articles')
  .where({ _extension: 'md' })
  .without(['body', 'excerpt'])
  .sort({ date: -1 })
  .findSurround(withoutTrailingSlash(route.path))
  , { default: () => [] })
</script>

<template>
  <UPage>
    <UPageHeader v-bind="page" :headline="headline">
      <template #headline>
        <UBreadcrumb :links="links" />
      </template>
    </UPageHeader>

    <UPage class="relative">
      <UPageBody prose >
        <ContentRenderer class="content-renderer" v-if="page && page.body" :value="page" />

        <hr v-if="surround?.length">

        <UContentSurround :surround="surround"  />
      </UPageBody>

      <template #right>
        <UContentToc v-if="page && page.body && page.body.toc && page.body.toc.links.length" :links="page.body.toc.links" />
      </template>
    </UPage>

  </UPage>
</template>

<style>

.content-renderer img{
  @apply mx-auto rounded-lg shadow-lg
}

</style>
