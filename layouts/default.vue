<template>
  <div>
    <AppHeader />
    <slot /> <!-- 渲染显示页面内容 -->
    <AppAside  @open-help="showHelp=true"/>
    <HelpMore v-if="showHelp" @close-help="showHelp=false"/>
    <AppFooter />
  </div>
</template>

<script setup>
const showHelp = ref(false);
</script>