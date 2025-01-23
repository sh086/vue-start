<template>
  <div class="guess-number-container">
    <div class="guess-number">
      <div class="header">
        <div class="game-count">第 {{ gameCount }} 局</div>
        <h1>猜数字游戏</h1>
        <div class="score">得分: {{ score }}</div>
      </div>
      <p>猜一个1到100之间的数字：</p>
      <p>提示：{{randomNumber}}</p>
      <input type="number" v-model="guess" placeholder="请输入你的猜测" class="input-field"/>
      <div class="button-container"> <!-- 添加按钮容器 -->
        <button v-if="isGameIng" @click="checkGuess" class="guess-button">猜一下</button>
        <button v-if="isGameSuccess" @click="moreGame" class="guess-button">再来一局</button>
      </div>
      <p v-if="message" class="message">{{ message }}</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

// 初始化游戏局数
let gameCount = ref(1);
// 初始化得分
let score = ref(0);
// 初始化显示按钮
let isGameIng = ref(true);
let isGameSuccess = ref(false);

// 生成一个1到100之间的随机数
const generateRandomNumber = () => Math.floor(Math.random() * 100) + 1;
// 初始化随机数
let randomNumber = ref(generateRandomNumber());
let guess = ref<number | null>(null);
let message = ref<string>('');


// 再来一局
const moreGame = () => {
  // 设置可见按钮
  isGameIng.value = true;
  isGameSuccess.value = false;
  // 重新生成随机数
  randomNumber = ref(generateRandomNumber());
  // 重置输入框和消息
  guess.value = null;
  message.value = '';
  gameCount.value++;
};

// 检查猜测
const checkGuess = () => {
  if (guess.value === null) {
    message.value = '请输入一个数字';
    return;
  }

  if (guess.value < 1 || guess.value > 100) {
    message.value = '请输入1到100之间的数字';
    return;
  }

  if (guess.value < randomNumber.value) {
    message.value = '太小了，再试一次';
  } else if (guess.value > randomNumber.value) {
    message.value = '太大了，再试一次';
  } else {
    message.value = '恭喜你，猜对了！';
    // 增加得分
    score.value += 10;
    // 设置可见按钮
    isGameIng.value = false;
    isGameSuccess.value = true;
  }
};
</script>

<style scoped>
.guess-number-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f0f0f0;
}

.guess-number {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-family: 'Arial', sans-serif;
  text-align: center;
  padding: 20px;
  width: 300px;
  border: 2px solid #4CAF50;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  background-color: #f9f9f9;
}

.header {
  display: flex;
  justify-content: space-between;
  width: 100%;
  margin-bottom: 20px;
  align-items: center; /* 添加对齐方式 */
}

.score, .game-count {
  font-size: 16px;
  color: #333;
}

.input-field {
  margin: 10px 0;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
  transition: border-color 0.3s;
}

.input-field:focus {
  border-color: #4CAF50;
  outline: none;
}

.button-container { /* 添加按钮容器样式 */
  display: flex;
  justify-content: center;
  gap: 10px; /* 添加间距 */
}

.guess-button {
  padding: 10px 20px;
  font-size: 16px;
  color: white;
  background-color: #4CAF50;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.guess-button:hover {
  background-color: #45a049;
}

.message {
  margin-top: 20px;
  font-size: 18px;
  color: #333;
}
</style>