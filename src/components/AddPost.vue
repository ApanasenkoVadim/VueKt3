<template>
  <div class="pf-outer">
    <div class="pf-inner">
      <input 
        type="text" 
        v-model="newPost" 
        placeholder="Новый пост ..." 
        class="pf-input"
      />
      <button @click="savePost" class="pf-btn">Добавить</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { usePostsStore } from '../stores/posts'

const store = usePostsStore()
const newPost = ref('')

const savePost = () => {
  if (newPost.value.trim()) {
    store.addPost({ title: newPost.value })
    newPost.value = ''
  }
}
</script>

<style scoped>
.pf-outer {
  padding: 12px;
  border-radius: 12px;
  margin-bottom: 24px;
  display: flex;
  justify-content: center;
}

.pf-inner {
  display: flex;
  gap: 10px;
  background-color: #fff;
  padding: 8px 10px 8px 16px;
  border-radius: 12px;
  border: 2px solid #3ea6ff;
  width: 100%;
  max-width: 520px;
}

.pf-input {
  flex: 1;
  padding: 10px 15px;
  border: 1.5px solid #e0e0e0;
  border-radius: 20px;
  font-size: 15px;
  outline: none;
  transition: border 0.2s;
}

.pf-btn {
  padding: 0 24px;
  background-color: #216cff;
  color: #fff;
  border: none;
  border-radius: 20px;
  cursor: pointer;
  font-size: 15px;
  font-weight: 500;
  transition: background 0.2s;
  height: 38px;
}
</style> 