<script lang="ts" setup>
import { makeFirstCharUpper } from '@/utils/helper'
import type { QueryBuilderParams } from '@nuxt/content/dist/runtime/types'

const { $notion } = useNuxtApp()
// const { data } = await useAsyncData('notion', () =>
//   $notion.getPageBlocks('c7ca4eca35ee4f8ebe2665a7be719c36')
// )
const { data } = await useAsyncData('notion2', () =>
  $notion.getPageTable('c7ca4eca35ee4f8ebe2665a7be719c36')
)

const { data: areas } = await useAsyncData('equal', () => {
  return queryContent('/areas').find()
})

// const directories = computed(() => {
//   return areas.map((d) => d._dir)
// })

useHead({
  title: 'Areas',
  meta: [
    {
      name: 'description',
      content:
        'Blow All the topics are listed on which either I have written a blog or will write a blog in near future.',
    },
  ],
  titleTemplate: "Boloko's areas - %s",
})
</script>
<template>
  <main class="container max-w-5xl mx-auto text-zinc-600">
    <MainHero title="Áreas de interesse" subtitle="Assuntos que estou sempre me atualizando" />
    <div class="flex flex-wrap px-6 mt-12 gap-3">
      <template v-for="topic in areas?.map((d) => d._dir)" :key="topic">
        <AreaCard :title="makeFirstCharUpper(topic)" />
      </template>
    </div>

    <div class="space-y-5 my-5">
      <!-- <template v-for="post in areas" :key="post.title">
        <ArchiveCard
          :path="post._path"
          :title="post.title"
          :date="post.date"
          :description="post.description"
          :image="post.image"
          :alt="post.alt"
          :ogImage="post.ogImage"
          :provider="post.provider"
          :tags="post.tags"
          :published="post.published"
        />
      </template> -->
      <FoodTable :items="data"></FoodTable>
    </div>
  </main>
</template>
