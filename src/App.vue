<template>
  <div class="chat-container">
    <div class="chat-header">
      <h1>PF_FE_VIRGO</h1>
      <p>简易聊天机器人</p>
    </div>
    
    <div class="chat-messages" ref="messagesContainer">
      <ChatMessage 
        v-for="(message, index) in messages" 
        :key="index"
        :content="message.content"
        :isUser="message.role === 'user'"
      />
    </div>
    
    <div class="chat-input">
      <textarea 
        v-model="userInput" 
        placeholder="输入您的问题..."
        @keyup.enter.ctrl="sendMessage"
        rows="1"
        ref="inputField"
      ></textarea>
      <button @click="sendMessage" :disabled="isLoading">
        <span v-if="!isLoading">发送</span>
        <span v-else>...</span>
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch, nextTick } from 'vue';
import ChatMessage from './components/ChatMessage.vue';

const messages = ref([
  { role: 'bot', content: '你好！我是Virgo助手，有什么可以帮到您的？' }
]);
const userInput = ref('');
const isLoading = ref(false);
const messagesContainer = ref(null);
const inputField = ref(null);

function sendMessage() {
  if (userInput.value.trim() === '' || isLoading.value) return;
  
  // 添加用户消息
  messages.value.push({ role: 'user', content: userInput.value });
  
  // 模拟API请求
  isLoading.value = true;
  setTimeout(() => {
    const botResponses = [
      "我理解您的问题，让我思考一下...",
      "这是一个有趣的问题！根据我的理解...",
      "非常感谢您的提问。以我的分析...",
      "我很高兴回答这个问题。简单来说...",
      "这个问题很有深度。从专业角度看..."
    ];
    
    // 随机选择一个回复
    const randomResponse = botResponses[Math.floor(Math.random() * botResponses.length)];
    messages.value.push({ role: 'bot', content: randomResponse });
    isLoading.value = false;
    userInput.value = '';
    
    // 聚焦输入框
    inputField.value.focus();
  }, 1000);
}

// 监听消息变化，自动滚动到底部
watch(messages, () => {
  nextTick(() => {
    if (messagesContainer.value) {
      messagesContainer.value.scrollTop = messagesContainer.value.scrollHeight;
    }
  });
}, { deep: true });

onMounted(() => {
  // 初始化时聚焦输入框
  inputField.value.focus();
});
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

body {
  background-color: #f5f5f5;
}

.chat-container {
  display: flex;
  flex-direction: column;
  max-width: 1000px;
  width: 100%;
  height: 100vh;
  margin: 0 auto;
  background-color: white;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  overflow: hidden;
}

.chat-header {
  padding: 20px;
  text-align: center;
  background-color: #343541;
  color: white;
}

.chat-header h1 {
  font-size: 1.8rem;
  margin-bottom: 8px;
}

.chat-header p {
  font-size: 1rem;
  opacity: 0.8;
}

.chat-messages {
  flex: 1;
  overflow-y: auto;
  padding: 20px;
  display: flex;
  flex-direction: column;
  background-color: #f9f9f9;
}

.chat-input {
  padding: 20px;
  border-top: 1px solid #e5e5e5;
  display: flex;
  align-items: center;
  background-color: white;
}

textarea {
  flex: 1;
  height: 50px;
  padding: 12px 15px;
  border: 1px solid #e5e5e5;
  border-radius: 8px;
  margin-right: 12px;
  resize: none;
  font-size: 1rem;
  max-height: 150px;
  overflow-y: auto;
  transition: border-color 0.3s;
}

textarea:focus {
  outline: none;
  border-color: #343541;
}

button {
  height: 50px;
  width: 80px;
  background-color: #343541;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #444654;
}

button:disabled {
  background-color: #6b6c7b;
  cursor: not-allowed;
}

/* 响应式设计 - PC端 */
@media (min-width: 769px) {
  .chat-container {
    height: 90vh;
    max-height: 800px;
    border-radius: 12px;
    margin: 2rem auto;
  }
  
  .chat-header h1 {
    font-size: 2rem;
  }
  
  .chat-header p {
    font-size: 1.1rem;
  }
  
  textarea {
    font-size: 1.1rem;
  }
  
  button {
    font-size: 1.1rem;
  }
}

/* 响应式设计 - 移动端 */
@media (max-width: 768px) {
  .chat-container {
    height: 100vh;
    width: 100%;
    max-width: none;
    border-radius: 0;
  }
  
  .chat-header h1 {
    font-size: 1.3rem;
  }
}
</style>
