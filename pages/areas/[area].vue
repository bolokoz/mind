<script lang="ts" setup>
const route = useRoute()

// take category from route params & make first char upper
const area = computed(() => {
  let name = route.params.area || ''
  let strName = ''

  if (name instanceof Array) strName = name.at(0) || ''
  else strName = name
  return strName
})

const { data } = await useAsyncData('areas', () => queryContent('/areas').find())

const formatedData = computed(() => {
  return data.value?.map((articles) => {
    return {
      path: articles._path,
      title: articles.title || 'no-title available',
      description: articles.description || 'no-descriptoin available',
      image: articles.image || '/nuxt-blog/no-image_cyyits.png',
      alt: articles.alt || 'no alter data available',
      ogImage: articles.ogImage || '/nuxt-blog/no-image_cyyits.png',
      provider: articles.provider,
      date: articles.date || 'not-date-available',
      tags: articles.tags || [],
      published: articles.published || false,
    }
  })
})

useHead({
  title: area.value,
  meta: [
    {
      name: 'description',
      content: `You will find all the ${area.value} related post here`,
    },
  ],
  titleTemplate: "Riyad's Blog - %s",
})
</script>
<template>
  <main class="container max-w-5xl mx-auto text-zinc-600">
    <AreaTopic />
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3">
      <template v-for="post in formatedData" :key="post.title">
        <BlogCard
          :path="post.path"
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
      </template>
      <template v-if="data?.length === 0">
        <BlogEmpty />
      </template>
    </div>
  </main>
</template>
