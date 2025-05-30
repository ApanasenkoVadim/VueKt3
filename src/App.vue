<script setup>
import { usePostsStore } from './stores/posts'
import { storeToRefs } from 'pinia'
import AddPost from './components/AddPost.vue'
import PostItem from './components/PostItem.vue'

const store = usePostsStore()
const { posts } = storeToRefs(store)
</script>

<template>
  <div class="app">
    <main class="main">
      <AddPost />
      <div class="posts-list">
        <PostItem v-for="post in posts" :key="post.id" :post="post" />
      </div>
    </main>
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
  background-color: #e3f2fd;
  color: #333;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding-top: 40px;
}

.app {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  padding: 0 20px;
}

.posts-list {
  display: flex;
  flex-direction: column;
  gap: 20px;
}
</style>
