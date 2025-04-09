<template>
  <div :class="['message', isUser ? 'user-message' : 'bot-message']">
    <div class="avatar" v-if="!isUser">
      <img src="../assets/grok-avatar.png" alt="Grok" class="avatar-img" />
    </div>
    <div class="message-content">
      <div v-if="featureType" class="feature-indicator">
        <span v-if="featureType === 'deepsearch'" class="feature-badge deepsearch">👁️ DeepSearch</span>
        <span v-if="featureType === 'think'" class="feature-badge think">💡 Think</span>
        <span v-if="featureType === 'editimage'" class="feature-badge editimage">✏️ Edit Image</span>
      </div>
      {{ content }}
    </div>
    <div class="avatar user-avatar" v-if="isUser">
      <img src="../assets/user-avatar.png" alt="User" class="avatar-img" />
    </div>
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
  },
  featureType: {
    type: String,
    default: null
  }
});
</script>

<style scoped>
.message {
  max-width: 80%;
  padding: 12px 0;
  margin-bottom: 16px;
  word-wrap: break-word;
  display: flex;
  align-items: flex-start;
  gap: 10px;
}

.message-content {
  padding: 12px 16px;
  border-radius: 16px;
  flex: 1;
  font-size: 1rem;
  line-height: 1.5;
}

.user-message {
  align-self: flex-end;
  flex-direction: row;
}

.user-message .message-content {
  background-color: #f0f0f0;
  color: #000;
  border-radius: 16px;
}

.bot-message {
  align-self: flex-start;
  flex-direction: row;
}

.bot-message .message-content {
  background-color: #fff;
  color: #000;
  border: 1px solid #e0e0e0;
  border-radius: 16px;
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
  overflow: hidden;
  background-color: #f0f0f0;
}

.avatar-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.feature-indicator {
  margin-bottom: 8px;
}

.feature-badge {
  display: inline-block;
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 0.8rem;
  font-weight: 500;
  margin-bottom: 8px;
}

.deepsearch {
  background-color: rgba(30, 136, 229, 0.1);
  color: #1e88e5;
}

.think {
  background-color: rgba(76, 175, 80, 0.1);
  color: #4caf50;
}

.editimage {
  background-color: rgba(156, 39, 176, 0.1);
  color: #9c27b0;
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
    padding: 8px 0;
  }
  
  .avatar {
    width: 30px;
    height: 30px;
  }
}
</style> 