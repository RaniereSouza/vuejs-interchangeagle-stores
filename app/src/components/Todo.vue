<script setup lang="ts">
  import { onMounted } from 'vue'

  const title = 'TODO App'
  const description = 'Description: TBD'

  onMounted(() => {
    document.title = title
  })
</script>

<template>
  <h1>{{ title }}</h1>
  <p>{{ description }}</p>
</template>

<style scoped>
  h1 {
    color: cornflowerblue;
  }
</style>
