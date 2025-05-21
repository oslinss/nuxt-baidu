<template>
  <MainContent/>
</template>

<script setup lang="ts">
// 将用户的 headers 和 cookies 转发到 `/api/cookies`
const requestFetch = useRequestFetch()
const { data } = useAsyncData(() => requestFetch('/api/cookies'))
</script>

<style scoped>
img {
  display: block;
  margin: 0 auto;
  width: 270px;
  height: 129px;
}
</style>