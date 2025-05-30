<template>
  <div class="post-item-wrapper">
    <div class="post-item">
      <div class="post-content">
        <p class="post-text">{{ post.title }}</p>
        <hr class="divider" />
        <div class="post-actions">
          <button @click="toggleComments" class="btn comments-btn">
            Комментарии
          </button>
          <span class="comments-count-label">
            Количество комментариев - {{ post.comments.length }}
          </span>
          <button @click="openEditModal" class="btn edit-btn">Изменить</button>
          <button @click="deletePost" class="btn delete-btn">Удалить</button>
        </div>
      </div>

      <div v-if="isShowComments" class="comments-section">
        <div class="add-comment">
          <input 
            type="text" 
            v-model="newComment" 
            placeholder="Новый комментарий ..." 
            class="comment-input"
          />
          <button @click="addComment" class="btn add-comment-btn">Добавить</button>
        </div>
        
        <div class="comments-list">
          <div v-for="comment in post.comments" :key="comment.id" class="comment-item">
            <p>{{ comment.text }}</p>
            <button @click="deleteComment(comment.id)" class="btn delete-comment-btn">Удалить</button>
          </div>
        </div>
        
        <button @click="toggleComments" class="btn hide-comments-btn">Скрыть</button>
      </div>

      <!-- Модальное окно для редактирования -->
      <div v-if="showEditModal" class="modal">
        <div class="modal-content">
          <textarea v-model="editedTitle" class="edit-textarea"></textarea>
          <div class="modal-actions">
            <button @click="saveEdit" class="btn save-btn">Сохранить</button>
            <button @click="showEditModal = false" class="btn cancel-btn">Закрыть</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { usePostsStore } from '../stores/posts'

const props = defineProps({
  post: {
    type: Object,
    required: true
  }
})

const store = usePostsStore()
const isShowComments = ref(false)
const showEditModal = ref(false)
const editedTitle = ref('')
const newComment = ref('')

const toggleComments = () => {
  isShowComments.value = !isShowComments.value
}

const openEditModal = () => {
  editedTitle.value = props.post.title
  showEditModal.value = true
}

const deletePost = () => {
  store.deletePost(props.post.id)
}

const deleteComment = (commentId) => {
  store.deleteComment(props.post.id, commentId)
}

const addComment = () => {
  if (newComment.value.trim()) {
    store.addComment(props.post.id, newComment.value)
    newComment.value = ''
  }
}

const saveEdit = () => {
  if (editedTitle.value.trim()) {
    store.updatePost(props.post.id, editedTitle.value)
    showEditModal.value = false
  }
}
</script>

<style scoped>
.post-item-wrapper {
  padding: 16px;
  border-radius: 12px;
  margin-bottom: 24px;
  display: flex;
  justify-content: center;
}

.post-item {
  background-color: #fff;
  border-radius: 12px;
  border: 2px solid #3ea6ff;
  box-shadow: 0 2px 12px rgba(62, 166, 255, 0.08);
  padding: 20px 24px 16px 24px;
  width: 100%;
  max-width: 520px;
}

.post-content {
  margin-bottom: 0;
}

.post-text {
  font-size: 16px;
  line-height: 1.5;
  margin-bottom: 10px;
  color: #222;
}

.divider {
  border: none;
  border-top: 1px solid #e3eaf3;
  margin: 10px 0 16px 0;
}

.post-actions {
  display: flex;
  align-items: center;
  gap: 12px;
  flex-wrap: wrap;
}

.btn {
  padding: 7px 18px;
  border: none;
  border-radius: 20px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  transition: background 0.2s, color 0.2s;
  outline: none;
}

.comments-btn {
  background: #2196f3;
  color: #fff;
}
.comments-btn:hover {
  background: #1976d2;
}

.edit-btn {
  background: #3ea6ff;
  color: #fff;
}
.edit-btn:hover {
  background: #1976d2;
}

.delete-btn {
  background: #ff4b8b;
  color: #fff;
}
.delete-btn:hover {
  background: #d81b60;
}

.comments-count-label {
  font-size: 13px;
  color: #888;
  margin: 0 8px;
}

.comments-section {
  margin-top: 18px;
  padding-top: 15px;
  border-top: 1px solid #e3eaf3;
}

.add-comment {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

.comment-input {
  flex: 1;
  padding: 8px 15px;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  font-size: 14px;
  outline: none;
}

.comment-input:focus {
  border-color: #2196F3;
}

.add-comment-btn {
  background-color: #2196F3;
  color: white;
  border-radius: 20px;
}
.add-comment-btn:hover {
  background: #1976d2;
}

.comments-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-bottom: 15px;
}

.comment-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 15px;
  background-color: #f5f5f5;
  border-radius: 8px;
  font-size: 14px;
}

.delete-comment-btn {
  background-color: #ff5252;
  color: white;
  padding: 4px 12px;
  font-size: 12px;
  border-radius: 20px;
}
.delete-comment-btn:hover {
  background: #d81b60;
}

.hide-comments-btn {
  background-color: #9e9e9e;
  color: white;
  width: 100%;
  border-radius: 20px;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 15px;
  width: 80%;
  max-width: 500px;
}

.edit-textarea {
  width: 100%;
  min-height: 100px;
  padding: 15px;
  margin-bottom: 15px;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  resize: vertical;
  font-size: 14px;
  outline: none;
}

.edit-textarea:focus {
  border-color: #2196F3;
}

.modal-actions {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
}

.save-btn {
  background-color: #4CAF50;
  color: white;
  border-radius: 20px;
}

.cancel-btn {
  background-color: #9e9e9e;
  color: white;
  border-radius: 20px;
}
</style> 