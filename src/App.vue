<template>
  <div id="app">
    <h1>体验语言的艺术与力量</h1>
    <div class="input-container">
      <input v-model="inputValue" placeholder="领略言辞的锋芒与魅力"/>
      <button @click="fireFull">火力全开</button>
      <button @click="spitFlowers">口吐芬芳</button>
    </div>
    
    <div id="history">
      <h2>历史记录</h2>
      <textarea v-model="formattedHistory" readonly></textarea>
    </div>
    
    <div v-if="copyMessage" :class="['copy-message', { show: copyMessage }]">
      {{ copyMessage }}
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      inputValue: '',
      history: [],
      copyMessage: ''
    };
  },
  computed: {
    formattedHistory() {
      return this.history.map((item, index) => `${index + 1}. ${item}`).join('\n');
    }
  },
  methods: {
   async fireFull() {
    try {
      const response = await axios.get(`${process.env.VUE_APP_API_BASE_URL}/zuan/fireFull`);
      this.inputValue = response.data.text;
      this.addToHistory(response.data.text);
      this.copyToClipboard(response.data.text);
    } catch (error) {
      console.error(error);
    }
  },
  async spitFlowers() {
    try {
      const response = await axios.get(`${process.env.VUE_APP_API_BASE_URL}/zuan/spitFlowers`);
      this.inputValue = response.data.text;
      this.addToHistory(response.data.text);
      this.copyToClipboard(response.data.text);
    } catch (error) {
      console.error(error);
    }
  },
    addToHistory(text) {
      this.history.push(text);
    },
    copyToClipboard(text) {
      navigator.clipboard.writeText(text).then(() => {
        this.copyMessage = '内容已复制到剪贴板!';
        setTimeout(() => {
          this.copyMessage = '';
        }, 3000); // 3秒后隐藏提示
      }).catch(err => {
        console.error('无法复制文本: ', err);
      });
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.input-container {
  display: flex;
  justify-content: center;
  align-items: center;
}

input {
  padding: 10px;
  margin: 10px;
  width: 600px;
}

button {
  padding: 10px 20px;
  margin: 10px;
  cursor: pointer;
}

#history {
  margin-top: 20px;
  text-align: left;
  width: 600px;
}

textarea {
  width: 100%;
  height: 300px;
  padding: 10px;
  margin: 10px;
  font-family: inherit;
  font-size: 16px;
  line-height: 1.5;
  resize: none;
}

.copy-message {
  position: fixed;
  top: -50px;
  left: 50%;
  transform: translateX(-50%);
  background-color: #4CAF50;
  color: white;
  padding: 15px;
  border-radius: 5px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  transition: top 0.5s ease-in-out;
}

.copy-message.show {
  top: 20px;
}
</style>