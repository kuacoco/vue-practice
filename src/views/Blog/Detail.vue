<script setup>
import BlogDetail from '@/views/Blog/components/BlogDetail.vue'
import BlogComment from '@/views/Blog/components/BlogComment.vue'
import BlogTOC from '@/views/Blog/components/BlogTOC.vue'
import Layout from '@/components/Layout.vue'
import { titleController } from '@/utils/index.js'
import { getBlog } from '@/api/blog.js'
import { useFetch } from '@/composables/fetch.js'
import { useMainScroll } from '@/composables/mainScroll.js'
import { useRoute } from 'vue-router'

const route = useRoute()
const { isLoading, data } = useFetch(fetchData)
useMainScroll('mainContainer')

async function fetchData() {
  const res = await getBlog(route.params.id)
  titleController.setRouteTitle(res.title)
  return res
}
</script>

<template>
  <Layout>
    <div class="main-container" v-loading="isLoading" ref="mainContainer">
      <BlogDetail :data v-if="data" />
      <BlogComment v-if="!isLoading" />
    </div>
    <template #right>
      <div class="right-container" v-loading="isLoading">
        <BlogTOC :toc="data.toc" v-if="data" />
      </div>
    </template>
  </Layout>
</template>

<style scoped lang="less">
.main-container {
  overflow-y: scroll;
  height: 100%;
  box-sizing: border-box;
  padding: 20px;
  position: relative;
  width: 100%;
  overflow-x: hidden;
  scroll-behavior: smooth;
}
.right-container {
  width: 300px;
  height: 100%;
  overflow-y: scroll;
  box-sizing: border-box;
  position: relative;
  padding: 20px;
}
</style>
