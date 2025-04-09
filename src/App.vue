<template>
  <div class="chat-container">
    <div class="chat-header">
      <div class="header-time">{{ currentTime }}</div>
      <div class="header-center">Q1 <span class="beta-tag">beta</span></div>
      <div class="header-icons">
        <span class="icon">⏱️</span>
        <span class="icon">✍️</span>
      </div>
    </div>
    
    <div class="chat-intro" v-if="messages.length === 1">
      <h1>去哪儿旅行 Q</h1>
      <p>Hi~我是Q， 是你的旅行 AI 小帮手。你今天想…</p>
      
      <div class="feature-card">
        <div class="feature-icon">💡 行程规划</div>
        <div class="feature-desc">阅读海量信息，协助您制定出最全面、精准的旅行方案。</div>
      </div>
      
      <div class="feature-card">
        <div class="feature-icon">👁️ 酒店比价</div>
        <div class="feature-desc">帮您全网比价，助您省钱省心。</div>
      </div>
      
      <div class="feature-card">
        <div class="feature-icon">✏️ 足迹地图</div>
        <div class="feature-desc">AI为您定制专属的旅行回忆地图，让您的旅程与众不同。</div>
      </div>
    </div>
    
    <div class="chat-messages" ref="messagesContainer" v-if="messages.length > 1">
      <ChatMessage 
        v-for="(message, index) in messages" 
        :key="index"
        :content="message.content"
        :isUser="message.role === 'user'"
        :featureType="message.featureType"
      />
    </div>
    
    <div class="chat-input">
      <div class="input-wrapper">
        <textarea 
          v-model="userInput" 
          placeholder="随便问点什么"
          @keyup.enter.ctrl="sendMessage"
          rows="1"
          ref="inputField"
        ></textarea>
        <div class="input-icons">
          <span class="tool-icon">📎</span>
          <span class="tool-icon">👁️</span>
          <span class="tool-icon">💡</span>
          <span class="tool-icon">✏️</span>
          <span class="mic-icon">🎤</span>
        </div>
      </div>
      
      <div class="nav-bar">
        <span class="nav-icon history-icon" @click="toggleHistoryList">🕒</span>
        <div class="history-dropdown" v-if="showHistoryList">
          <div class="history-title">最近浏览</div>
          <div 
            v-for="(item, index) in historyItems" 
            :key="index" 
            class="history-item"
            @click="selectHistoryItem(item)"
          >
            {{ item.title }}
          </div>
        </div>
        <span class="nav-icon">🔍</span>
        <span class="nav-icon-center">G</span>
        <span class="nav-icon">▶️</span>
        <span class="nav-icon">🔔<span class="badge">2</span></span>
        <span class="nav-icon">✉️</span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch, nextTick, computed } from 'vue';
import ChatMessage from './components/ChatMessage.vue';

const messages = ref([
  { role: 'bot', content: '你好！我是Grok助手，有什么可以帮到您的？' }
]);

const userInput = ref('');
const isLoading = ref(false);
const messagesContainer = ref(null);
const inputField = ref(null);
const showHistoryList = ref(false);
const historyItems = ref([
  { id: 1, title: '北京3日游攻略' },
  { id: 2, title: '上海必去景点' },
  { id: 3, title: '杭州西湖酒店推荐' },
  { id: 4, title: '三亚海滩度假村' }
]);

const currentTime = computed(() => {
  const now = new Date();
  return `${now.getHours()}:${now.getMinutes().toString().padStart(2, '0')}`;
});

function toggleHistoryList() {
  showHistoryList.value = !showHistoryList.value;
}

function selectHistoryItem(item) {
  // 处理选中历史记录项的逻辑
  userInput.value = `查看${item.title}的详细信息`;
  showHistoryList.value = false;
}

function sendMessage() {
  if (userInput.value.trim() === '' || isLoading.value) return;
  
  // 添加用户消息
  messages.value.push({ role: 'user', content: userInput.value });
  
  // 模拟API请求
  isLoading.value = true;
  
  // 检测是否需要使用特定功能
  const input = userInput.value.toLowerCase();
  let featureType = null;
  
  if (input.includes('搜索') || input.includes('查找') || input.includes('search')) {
    featureType = 'deepsearch';
  } else if (input.includes('计算') || input.includes('分析') || input.includes('think') || 
            input.includes('思考') || input.includes('编程') || input.includes('代码')) {
    featureType = 'think';
  } else if (input.includes('图片') || input.includes('照片') || input.includes('image') || 
            input.includes('编辑') || input.includes('edit')) {
    featureType = 'editimage';
  }
  
  setTimeout(() => {
    let response;
    
    if (featureType === 'deepsearch') {
  response = "💡 行程规划：阅读海量信息，协助您制定出最全面、精准的旅行方案。";
      } else if (featureType === 'think') {
        response = "👁️ 酒店比价：帮您全网比价，助您省钱省心。";
      } else if (featureType === 'editimage') {
        response = "✏️ 足迹地图：AI为您定制专属的旅行回忆地图，让您的旅程与众不同。";
          } else {
      const botResponses = [
        "我理解您的问题，让我思考一下...",
        "这是一个有趣的问题！根据我的理解...",
        "非常感谢您的提问。以我的分析...",
        "我很高兴回答这个问题。简单来说...",
        "这个问题很有深度。从专业角度看..."
      ];
      response = botResponses[Math.floor(Math.random() * botResponses.length)];
    }
    
    messages.value.push({ 
      role: 'bot', 
      content: response,
      featureType: featureType
    });
    
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
  width: 100%;
  height: 100vh;
  margin: 0 auto;
  background-color: white;
  overflow: hidden;
}

.chat-header {
  padding: 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #ffffff;
  border-bottom: 1px solid #f0f0f0;
}

.header-time {
  font-weight: bold;
  font-size: 1.1rem;
}

.header-center {
  font-weight: bold;
  font-size: 1.2rem;
}

.beta-tag {
  color: #1e88e5;
  font-weight: normal;
}

.header-icons {
  display: flex;
  gap: 15px;
}

.chat-intro {
  flex: 1;
  padding: 20px;
  overflow-y: auto;
}

.chat-intro h1 {
  font-size: 2rem;
  margin-bottom: 20px;
  font-weight: bold;
}

.chat-intro p {
  font-size: 1.5rem;
  margin-bottom: 30px;
  color: #666;
}

.feature-card {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 12px;
  margin-bottom: 20px;
}

.feature-icon {
  font-size: 1.2rem;
  font-weight: 600;
  margin-bottom: 10px;
}

.feature-desc {
  font-size: 1rem;
  color: #555;
  line-height: 1.5;
}

.chat-messages {
  flex: 1;
  overflow-y: auto;
  padding: 20px;
  display: flex;
  flex-direction: column;
  background-color: #fff;
}

.chat-input {
  padding: 10px;
  display: flex;
  flex-direction: column;
  background-color: #f9f9f9;
}

.input-wrapper {
  display: flex;
  align-items: center;
  background-color: #f0f0f0;
  padding: 10px 15px;
  border-radius: 20px;
  margin-bottom: 15px;
}

textarea {
  flex: 1;
  border: none;
  background: transparent;
  padding: 8px;
  resize: none;
  font-size: 1rem;
  max-height: 150px;
  overflow-y: auto;
}

textarea:focus {
  outline: none;
}

.input-icons {
  display: flex;
  gap: 10px;
}

.tool-icon {
  cursor: pointer;
  font-size: 1.2rem;
}

.mic-icon {
  cursor: pointer;
  font-size: 1.2rem;
  margin-left: 10px;
}

.nav-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 20px;
  position: relative;
}

.nav-icon {
  font-size: 1.5rem;
  cursor: pointer;
  position: relative;
}

.history-icon {
  color: #1e88e5;
}

.history-dropdown {
  position: absolute;
  bottom: 70px;
  left: 10px;
  width: 200px;
  background-color: white;
  border-radius: 10px;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.2);
  padding: 10px;
  z-index: 100;
}

.history-title {
  font-weight: bold;
  padding-bottom: 8px;
  margin-bottom: 8px;
  border-bottom: 1px solid #eee;
}

.history-item {
  padding: 8px;
  cursor: pointer;
  border-radius: 5px;
}

.history-item:hover {
  background-color: #f5f5f5;
}

.nav-icon-center {
  width: 50px;
  height: 50px;
  background-color: #000;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-weight: bold;
  font-size: 1.2rem;
}

.badge {
  position: absolute;
  top: -5px;
  right: -5px;
  background-color: #1e88e5;
  color: white;
  font-size: 0.7rem;
  padding: 2px 5px;
  border-radius: 50%;
}

/* 响应式设计 - 移动端 */
@media (max-width: 768px) {
  .chat-intro h1 {
    font-size: 1.8rem;
  }
  
  .chat-intro p {
    font-size: 1.2rem;
  }
}
</style>
