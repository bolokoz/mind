<script setup lang="ts">
import { makeFirstCharUpper } from '@/utils/helper'

const route = useRoute()

// take category from route params & make first char upper
const resource = computed(() => {
  let name = route.params.resource || ''
  let strName = ''

  if (name instanceof Array) strName = name.at(0) || ''
  else strName = name
  return makeFirstCharUpper(strName)
})
</script>

<template>
  <div class="container mx-auto">
    <div class="p-6 my-4 mx-3 rounded-md bg-gray-200">
      <h1 class="text-black font-semibold leading-tight text-4xl md:text-5xl my-5">
        Resource: #{{ resource }}
      </h1>
    </div>
  </div>
</template>
