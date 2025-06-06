<template>
    <div class="task-login-wrapper">
      <!-- 修改为背景图片 -->
      <div class="task-bg-image"></div>
      
      <!-- 居中容器 -->
      <div class="task-center-container">
        <!-- 登录表单 -->
        <div class="glass-task-form">
          <!-- 左侧宣传语区域 -->
          <div class="promo-section">
            <h2>任务备忘录系统</h2>
            <div class="divider"></div>
            <p class="slogan">"让每一刻都高效有序"</p>
            
            <p class="welcome-text">欢迎使用任务备忘录系统，您的专属时间管理专家！在这里，您可以：</p>
            
            <ul class="features">
              <li>📝 <strong>轻松记录</strong> - 快速添加任务，不错过任何重要事项</li>
              <li>✅ <strong>清晰管理</strong> - 直观标记完成状态，掌控工作进度</li>
              <li>🔍 <strong>智能筛选</strong> - 按状态分类查看，聚焦当下重点</li>
            </ul>
            
            <p class="slogan">"从繁杂到简单，从忙乱到从容"</p>
            
            <p class="conclusion">让我们帮您把碎片化的待办事项转化为清晰可执行的任务流，在高效完成工作的同时，享受井然有序的生活节奏。开始规划您的完美一天吧！</p>
          </div>
          
          <!-- 右侧登录表单 -->
          <div class="form-section">
            <h2 class="task-title">用户登录</h2>
            <div class="task-divider"></div>
            
            <form @submit.prevent="login" class="task-form">
              <div class="input-group">
                <input v-model="username" type="text" placeholder="用户名" required class="task-input">
                <i class="icon-user">👤</i>
              </div>
              
              <div class="input-group">
                <input v-model="password" type="password" placeholder="密码" required class="task-input">
                <i class="icon-lock">🔒</i>
              </div>
              
              <button type="submit" class="task-submit-btn">
                <span>登 录</span>
                <div class="task-btn-animation"></div>
              </button>
              
              <div class="register-link">
                <span>还没有账号？</span>
                <a @click="goToRegister">立即注册</a>
              </div>
            </form>
            
            <div class="task-footer">
              <span>高效管理您的每日任务</span>
              <div class="floating-dots">
                <span style="--i:11"></span>
                <span style="--i:12"></span>
                <span style="--i:24"></span>
                <span style="--i:10"></span>
                <span style="--i:14"></span>
                <span style="--i:23"></span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import { useAuthStore } from '../../store';
  import { useRouter } from 'vue-router';
  import axios from 'axios';
  
  const authStore = useAuthStore();
  const router = useRouter();
  
  const username = ref('');
  const password = ref('');
  
  const login = async () => {
    try {
      const response = await axios.post('http://localhost:5000/api/login', {
        username: username.value,
        password: password.value
      });
  
      const { token } = response.data;
      const user = { id: 1, name: username.value };
      authStore.login(user, token);
      router.push('/home');
    } catch (error) {
      console.error('登录错误:', error);
      alert('登录失败，请检查用户名和密码');
    }
  };
  
  const goToRegister = () => {
    router.push('/register');
  };
  </script>
  
  <style scoped>
  /* 布局样式 */
  .task-login-wrapper {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
  }
  
  .task-center-container {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
    box-sizing: border-box;
  }
  
  /* 玻璃态表单样式 - 修改为长方形 */
  .glass-task-form {
    position: relative;
    width: 900px; /* 加宽表单 */
    height: 600px; /* 设置固定高度 */
    display: flex;
    background: rgba(255, 255, 255, 0.15);
    backdrop-filter: blur(10px);
    border-radius: 16px;
    box-shadow: 0 8px 32px rgba(31, 38, 135, 0.37);
    border: 1px solid rgba(255, 255, 255, 0.18);
    overflow: hidden;
    z-index: 1;
  }
  
  /* 左侧宣传语区域 */
  .promo-section {
    flex: 1;
    padding: 10px 20px;
    color: white;
    overflow-y: auto;
  }
  
  .promo-section h2 {
    font-size: 2rem;
    margin-bottom: 10px;
  }
  
  .promo-section h3 {
    font-size: 1.5rem;
    margin-bottom: 20px;
    font-weight: 500;
  }
  
  .divider {
    height: 2px;
    background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.7), transparent);
    margin: 20px 0;
  }
  
  .slogan {
    font-style: italic;
    font-size: 1.2rem;
    margin: 25px 0;
    text-align: center;
  }
  
  .welcome-text {
    margin-bottom: 20px;
    line-height: 1.6;
  }
  
  .features {
    margin: 25px 0;
    padding-left: 20px;
  }
  
  .features li {
    margin-bottom: 15px;
    line-height: 1.6;
  }
  
  .conclusion {
    margin-top: 30px;
    line-height: 1.6;
  }
  
  /* 右侧登录表单区域 */
  .form-section {
    width: 380px;
    padding: 40px;
    display: flex;
    flex-direction: column;
  }
  
  /* 背景视频 */
  .task-bg-image {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-image: url('@/assets/1.png');
    background-size: cover;
    background-position: center;
    z-index: -1;
  }
  
  /* 移除背景视频样式 */
  .task-bg-video {
    display: none;
  }
  
  /* 修改玻璃态表单样式为黑色磨砂玻璃 */
  .glass-task-form {
    position: relative;
    width: 900px; /* 加宽表单 */
    height: 600px; /* 设置固定高度 */
    display: flex;
    background: rgba(58, 56, 56, 0.6); /* 修改为黑色磨砂背景 */
    backdrop-filter: blur(10px);
    border-radius: 16px;
    box-shadow: 0 8px 32px rgba(215, 218, 167, 0.37);
    border: 1px solid rgba(255, 255, 255, 0.18);
    overflow: hidden;
    z-index: 1;
  }
  
  /* 左侧宣传语区域 */
  .promo-section {
    flex: 1;
    padding: 10px 20px;
    color: white;
    overflow-y: auto;
  }
  
  .promo-section h2 {
    font-size: 2rem;
    margin-bottom: 10px;
  }
  
  .promo-section h3 {
    font-size: 1.5rem;
    margin-bottom: 20px;
    font-weight: 500;
  }
  
  .divider {
    height: 2px;
    background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.7), transparent);
    margin: 25px 0;
  }
  
  .slogan {
    font-style: italic;
    font-size: 1.2rem;
    margin: 25px 0;
    text-align: center;
  }
  
  .welcome-text {
    margin-bottom: 20px;
    line-height: 1.6;
  }
  
  .features {
    margin: 25px 0;
    padding-left: 20px;
  }
  
  .features li {
    margin-bottom: 15px;
    line-height: 1.6;
  }
  
  .conclusion {
    margin-top: 30px;
    line-height: 1.6;
  }
  
  /* 右侧登录表单区域 */
  .form-section {
    width: 380px;
    padding: 40px;
    display: flex;
    flex-direction: column;
  }
  
  /* 标题样式 */
  .task-title {
    color: white;
    text-align: center;
    margin-bottom: 30px;
    font-size: 2.2rem;
    font-weight: 600;
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    letter-spacing: 2px;
  }
  
  /* 分隔线 */
  .task-divider {
    height: 2px;
    background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.7), transparent);
    margin: 25px 0;
  }
  
  /* 输入框样式 */
  .task-input {
    width: 85%;
    padding: 12px 15px 12px 40px;
    margin-bottom: 20px;
    border: none;
    border-radius: 25px;
    background: rgba(255, 255, 255, 0.8);
    font-size: 1rem;
    color: #112734;
    transition: all 0.3s ease;
  }
  
  .task-input:focus {
    outline: none;
    background: rgba(255, 255, 255, 0.95);
    box-shadow: 0 0 10px rgba(0, 149, 255, 0.3);
  }
  
  /* 输入组样式 */
  .input-group {
    position: relative;
  }
  
  .input-group i {
    position: absolute;
    left: 15px;
    top: 12px;
    color: #112734;
    font-size: 1.1rem;
  }
  
  /* 提交按钮样式 */
  .task-submit-btn {
    position: relative;
    width: 100%;
    padding: 12px;
    margin-top: 20px;
    background: linear-gradient(45deg, #0095ff, #00d4ff);
    color: white;
    border: none;
    border-radius: 25px;
    font-size: 1.1rem;
    font-weight: 600;
    cursor: pointer;
    overflow: hidden;
    transition: all 0.3s ease;
    z-index: 1;
  }
  
  .task-submit-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 5px 15px rgba(0, 149, 255, 0.4);
  }
  
  /* 按钮动画 */
  .task-btn-animation {
    position: absolute;
    top: -100%;
    left: 0;
    width: 100%;
    height: 300%;
    background: linear-gradient(45deg, transparent, rgba(255, 255, 255, 0.3), transparent);
    transform: rotate(45deg);
    transition: all 0.5s ease;
    z-index: -1;
  }
  
  .task-submit-btn:hover .task-btn-animation {
    top: -20%;
  }
  
  /* 注册链接样式 */
  .register-link {
    margin-top: 20px;
    text-align: center;
    color: rgba(255, 255, 255, 0.8);
  }
  
  .register-link a {
    color: white;
    font-weight: bold;
    text-decoration: none;
    margin-left: 8px;
    cursor: pointer;
    transition: all 0.3s;
  }
  
  .register-link a:hover {
    text-decoration: underline;
    color: #00d4ff;
  }
  
  /* 页脚样式 */
  .task-footer {
    margin-top: auto;
    text-align: center;
    color: rgba(255, 255, 255, 0.8);
    font-size: 0.9rem;
    position: relative;
  }
  
  /* 浮动点动画 */
  .floating-dots {
    position: absolute;
    bottom: -50px;
    left: 0;
    width: 100%;
    height: 100px;
    overflow: hidden;
  }
  
  .floating-dots span {
    position: absolute;
    bottom: 0;
    background: rgba(255, 255, 255, 0.6);
    border-radius: 50%;
    animation: dot-float 15s linear infinite;
    opacity: 0;
  }
  
  @keyframes dot-float {
    0% {
      transform: translateY(0);
      opacity: 0;
      width: 5px;
      height: 5px;
    }
    10% {
      opacity: 0.6;
    }
    30% {
      width: calc(var(--i) * 0.2px);
      height: calc(var(--i) * 0.2px);
    }
    100% {
      transform: translateY(-100vh);
      opacity: 0;
    }
  }
  
  /* 响应式设计 */
  @media (max-width: 992px) {
    .glass-task-form {
      width: 90%;
      height: auto;
      flex-direction: column;
    }
    
    .form-section {
      width: 100%;
    }
    
    .promo-section {
      padding: 30px;
    }
  }
  
  @media (max-width: 480px) {
    .glass-task-form {
      width: 95%;
      padding: 20px;
    }
    
    .task-title {
      font-size: 1.8rem;
    }
    
    .promo-section h2 {
      font-size: 1.6rem;
    }
    
    .promo-section h3 {
      font-size: 1.2rem;
    }
  }
  </style>