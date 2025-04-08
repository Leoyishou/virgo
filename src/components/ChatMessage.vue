<template>
  <div :class="['message', isUser ? 'user-message' : 'bot-message']">
    <div class="avatar" v-if="!isUser">V</div>
    <div class="message-content">{{ content }}</div>
    <div class="avatar" v-if="isUser">U</div>
  </div>
</template>

<script setup>
defineProps({
  content: {
    type: String,
    required: true
  },
  isUser: {
    type: Boolean,
    default: false
  }
});
</script>

<style scoped>
.message {
  max-width: 80%;
  padding: 12px 16px;
  border-radius: 12px;
  line-height: 1.5;
  margin-bottom: 16px;
  word-wrap: break-word;
  display: flex;
  align-items: flex-start;
  gap: 10px;
}

.message-content {
  padding: 12px 16px;
  border-radius: 10px;
  flex: 1;
}

.user-message {
  align-self: flex-end;
  flex-direction: row;
}

.user-message .message-content {
  background-color: #343541;
  color: white;
  text-align: right;
}

.bot-message {
  align-self: flex-start;
  flex-direction: row;
}

.bot-message .message-content {
  background-color: #f7f7f8;
  color: #000;
  border: 1px solid #e5e5e5;
}

.avatar {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  flex-shrink: 0;
}

.user-message .avatar {
  background-color: #6b6c7b;
  color: white;
}

.bot-message .avatar {
  background-color: #10a37f;
  color: white;
}

@media (min-width: 769px) {
  .message {
    max-width: 70%;
    margin-bottom: 24px;
  }
  
  .message-content {
    font-size: 1.05rem;
    line-height: 1.6;
  }
  
  .avatar {
    width: 40px;
    height: 40px;
  }
}

@media (max-width: 768px) {
  .message {
    max-width: 90%;
    padding: 8px 12px;
  }
  
  .avatar {
    width: 30px;
    height: 30px;
    font-size: 0.8rem;
  }
}
</style> 