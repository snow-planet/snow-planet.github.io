<template>
    <div class="task-register-wrapper">
      <!-- 背景视频 -->
      <video autoplay muted loop class="task-bg-video">
        <source src="@/assets/video.mp4" type="video/mp4">
      </video>
      
      <!-- 居中容器 -->
      <div class="task-center-container">
        <!-- 注册表单 -->
        <div class="glass-task-form">
          <h2 class="task-title">注册账号</h2>
          <div class="task-divider"></div>
          
          <form @submit.prevent="register" class="task-form">
            <div class="input-group">
              <input v-model="username" type="text" placeholder="用户名" required class="task-input">
              <i class="icon-user">👤</i>
            </div>
            
            <div class="input-group">
              <input v-model="password" type="password" placeholder="密码(至少6位)" required class="task-input">
              <i class="icon-lock">🔒</i>
            </div>
            
            <button type="submit" class="task-submit-btn">
              <span>注 册</span>
              <div class="task-btn-animation"></div>
            </button>
            
            <div class="login-link">
              <span>已有账号？</span>
              <a @click="goToLogin">立即登录</a>
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
  
  const register = async () => {
    try {
      // 验证输入
      if (!username.value || !password.value) {
        alert('用户名和密码是必填的');
        return;
      }
      
      if (password.value.length < 6) {
        alert('密码长度至少为6个字符');
        return;
      }
  
      await axios.post('http://localhost:5000/api/register', {
        username: username.value,
        password: password.value
      });
  
      alert('注册成功！请登录');
      router.push('/login');
    } catch (error) {
      console.error('注册错误:', error);
      if (error.response && error.response.data.message) {
        alert(error.response.data.message);
      } else {
        alert('注册过程中发生错误');
      }
    }
  };
  
  const goToLogin = () => {
    router.push('/login');
  };
  </script>
  
  <style scoped>
  /* 布局样式 */
  .task-register-wrapper {
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
  
  /* 玻璃态表单样式 */
  .glass-task-form {
    position: relative;
    width: 380px;
    padding: 40px;
    background: rgba(255, 255, 255, 0.15);
    backdrop-filter: blur(10px);
    border-radius: 16px;
    box-shadow: 0 8px 32px rgba(31, 38, 135, 0.37);
    border: 1px solid rgba(255, 255, 255, 0.18);
    overflow: hidden;
    z-index: 1;
  }
  
  /* 背景视频 */
  .task-bg-video {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    z-index: -1;
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
  
  /* 登录链接样式 */
  .login-link {
    margin-top: 20px;
    text-align: center;
    color: rgba(255, 255, 255, 0.8);
  }
  
  .login-link a {
    color: white;
    font-weight: bold;
    text-decoration: none;
    margin-left: 8px;
    cursor: pointer;
    transition: all 0.3s;
  }
  
  .login-link a:hover {
    text-decoration: underline;
    color: #00d4ff;
  }
  
  /* 页脚样式 */
  .task-footer {
    margin-top: 30px;
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
  @media (max-width: 480px) {
    .glass-task-form {
      width: 90%;
      padding: 30px 20px;
    }
    
    .task-title {
      font-size: 1.8rem;
    }
  }
  </style>