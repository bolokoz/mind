<script lang="ts" setup>
const route = useRoute()

// take category from route params & make first char upper
const resource = computed(() => {
  let name = route.params.resource || ''
  let strName = ''

  if (name instanceof Array) strName = name.at(0) || ''
  else strName = name
  return strName
})

const { data } = await useAsyncData('resources', () => queryContent(route.path).find())

const articles = await queryContent(route.path).find()

// const formatedData = computed(() => {
//   return data.value?.map((articles) => {
//     return {
//       path: articles._path,
//       title: articles.title || 'no-title available',
//       description: articles.description || 'no-descriptoin available',
//       image: articles.image || '/nuxt-blog/no-image_cyyits.png',
//       alt: articles.alt || 'no alter data available',
//       ogImage: articles.image || '/nuxt-blog/no-image_cyyits.png',
//       provider: articles.provider,
//       date: articles.date || 'not-date-available',
//       tags: articles.tags || [],
//       published: articles.published || false,
//     }
//   })
// })

useHead({
  title: resource.value,
  meta: [
    {
      name: 'description',
      content: `You will find all the ${resource.value} related post here`,
    },
  ],
  titleTemplate: "Boloko's resources - %s",
})
</script>

<template>
  <main class="container max-w-5xl mx-auto text-zinc-600">
    <ResourceTopic />
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3">
      <template v-for="post in data" :key="post.title">
        <MainMulticard v-bind="post" />
      </template>
      <template v-if="data?.length === 0">
        <BlogEmpty />
      </template>
    </div>
  </main>
</template>
