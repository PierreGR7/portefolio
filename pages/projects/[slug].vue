<script setup>
const route = useRoute()
import { withoutTrailingSlash } from 'ufo'


onMounted(() => {
  MathJax.typeset();
});


const { data: page } = await useAsyncData(route.path, () => queryContent(route.path).findOne())

const links = [{
  label: 'Home',
  icon: 'i-heroicons-home',
  to: '/'
}, {
  label: 'Projects',
  to: '/#projects',
  icon: 'i-heroicons-square-3-stack-3d'
}, {
  label: page.value ? page.value.title : 'Random',
  icon: "material-symbols:article"
}]


const headline = computed(() => findPageHeadline(page.value))

const { data: surround } = await useAsyncData(`${route.path}-surround`, () => queryContent('/projects')
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
    <!-- <UPageBody prose>
      <ContentRenderer v-if="page && page.body" :value="page" />
    </UPageBody> -->

    <UPage class="relative">
      <UPageBody prose >
        <ContentRenderer v-if="page && page.body" :value="page" />

        <hr v-if="surround?.length">

        <UContentSurround :surround="surround"  />
      </UPageBody>

      <template #right>
        <UContentToc v-if="page && page.body && page.body.toc" :links="page.body.toc.links" />
      </template>
    </UPage>

  </UPage>
</template>
