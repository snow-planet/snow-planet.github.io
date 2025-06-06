<template>
  <div class="task-container font-inter">
    <!-- 背景视频/图片 -->
    <video 
      src="@/assets/video.mp4" 
      class="task-bg-video"
      autoplay
      loop
      muted
      playsinline
    ></video>
    
    <!-- 顶部导航栏 -->
    <header class="task-header glass-effect-dark">
      <div class="header-content">
        <div class="logo-area">
          <i class="logo-icon fas fa-tasks text-primary"></i>
          <h1 class="task-logo">任务备忘录系统</h1>
        </div>
        <!-- 移动端汉堡菜单 -->
        <button v-if="isMobile" class="mobile-menu-btn" @click="toggleMenu">
          <i class="fas fa-bars"></i>
        </button>
        <nav :class="{ 'mobile-menu-open': isMobile && menuOpen }">
          <div class="nav-buttons">
            <router-link 
              to="/home" 
              class="mode-btn px-4 py-2 rounded-lg"
              :class="{ 
                'active bg-primary text-white': $route.name === 'TaskDetail',
                'bg-white/10 hover:bg-white/20': $route.name !== 'TaskDetail'
              }"
              @click="closeMenu"
            >
              <i class="fa fa-th-large mr-2"></i>
              任务展示
            </router-link>
            <router-link 
              to="/home/add" 
              class="mode-btn px-4 py-2 rounded-lg"
              :class="{ 
                'active bg-primary text-white': $route.name === 'TaskForm',
                'bg-white/10 hover:bg-white/20': $route.name !== 'TaskForm'
              }"
              @click="closeMenu"
            >
              <i class="fa fa-plus mr-2"></i>
              任务添加
            </router-link>
            <router-link 
              to="/home/list" 
              class="mode-btn px-4 py-2 rounded-lg"
              :class="{ 
                'active bg-primary text-white': $route.name === 'TaskList',
                'bg-white/10 hover:bg-white/20': $route.name !== 'TaskList'
              }"
              @click="closeMenu"
            >
              <i class="fa fa-list mr-2"></i>
              任务查找
            </router-link>
            <button 
              class="logout-btn px-4 py-2 rounded-lg bg-white/10 hover:bg-white/20 transition-all"
              @click="handleLogout"
            >
              <i class="fa fa-sign-out-alt mr-2"></i>
              退出登录
            </button>
          </div>
        </nav>
      </div>
    </header>
    
    <!-- 统计卡片区域 -->
    <div class="stats-cards">
      <div class="stat-card glass-effect task-card-hover">
        <div class="flex items-center justify-between">
          <div>
            <h3 class="text-muted text-sm">今日任务</h3>
            <p class="text-2xl font-bold text-dark mt-1">10</p>
            <p class="text-success text-sm mt-2 flex items-center">
              <i class="fa fa-arrow-up mr-1"></i> 较昨日增加 2
            </p>
          </div>
          <div class="w-12 h-12 rounded-full bg-primary-light flex items-center justify-center">
            <i class="fa fa-calendar-check-o text-primary text-xl"></i>
          </div>
        </div>
      </div>
      <div class="stat-card glass-effect task-card-hover">
        <div class="flex items-center justify-between">
          <div>
            <h3 class="text-muted text-sm">进行中</h3>
            <p class="text-2xl font-bold text-dark mt-1">5</p>
            <p class="text-warning text-sm mt-2 flex items-center">
              <i class="fa fa-minus mr-1"></i> 较昨日减少 1
            </p>
          </div>
          <div class="w-12 h-12 rounded-full bg-primary-light flex items-center justify-center">
            <i class="fa fa-spinner text-primary text-xl"></i>
          </div>
        </div>
      </div>
      <div class="stat-card glass-effect task-card-hover">
        <div class="flex items-center justify-between">
          <div>
            <h3 class="text-muted text-sm">已完成</h3>
            <p class="text-2xl font-bold text-dark mt-1">3</p>
            <p class="text-success text-sm mt-2 flex items-center">
              <i class="fa fa-arrow-up mr-1"></i> 较昨日增加 5
            </p>
          </div>
          <div class="w-12 h-12 rounded-full bg-primary-light flex items-center justify-center">
            <i class="fa fa-check-circle text-primary text-xl"></i>
          </div>
        </div>
      </div>
    </div>
    
    <!-- 主要内容区域 - 使用 router-view 显示子路由 -->
    <main class="task-main">
      <div class="main-content glass-effect">
        <router-view></router-view>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const menuOpen = ref(false);
const windowWidth = ref(window.innerWidth);

const isMobile = computed(() => windowWidth.value < 768);

const handleLogout = () => {
  localStorage.removeItem('token');
  localStorage.removeItem('username');
  router.push('/login');
};

const toggleMenu = () => {
  menuOpen.value = !menuOpen.value;
};

const closeMenu = () => {
  menuOpen.value = false;
};

const handleResize = () => {
  windowWidth.value = window.innerWidth;
  if (windowWidth.value >= 768) {
    menuOpen.value = false;
  }
};

onMounted(() => {
  window.addEventListener('resize', handleResize);
});

onUnmounted(() => {
  window.removeEventListener('resize', handleResize);
});
</script>

<style scoped>
/* 引入 Inter 字体和 Font Awesome */
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
@import url('https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css');

/* 基础样式 */
.task-container {
  position: relative;
  min-height: 100vh;
  width: 100%;
  overflow-x: hidden;
  font-family: 'Inter', sans-serif;
  background: rgba(0, 0, 0, 0.6);
  backdrop-filter: blur(10px);
  margin: 0;
  padding: 0;
}

/* 背景视频样式 */
.task-bg-video {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  z-index: -1;
  filter: brightness(0.5);
}

/* 顶部导航栏 */
.task-header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: 60px;
  background: rgba(29, 33, 41, 0.8);
  backdrop-filter: blur(10px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  display: flex;
  align-items: center;
  padding: 0 20px;
  z-index: 100;
}

.header-content {
  width: 100%;
  max-width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 0 auto;
}

.logo-area {
  display: flex;
  align-items: center;
}

.task-logo {
  color: white;
  font-size: 24px;
  margin: 0;
  font-weight: 600;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 200px;
}

.logo-icon {
  margin-right: 10px;
  font-size: 1.8rem;
}

/* 导航按钮区域 */
.nav-buttons {
  display: flex;
  align-items: center;
  gap: 10px;
  flex-wrap: nowrap;
}

.mode-btn {
  padding: 10px 20px;
  border-radius: 15px;
  color: white;
  border: none;
  cursor: pointer;
  font-size: 16px;
  transition: all 0.3s;
  text-decoration: none;
  white-space: nowrap;
  display: flex;
  align-items: center;
}

.logout-btn {
  padding: 5px 10px;
  border-radius: 15px;
  color: white;
  background-color: #F53F3F;
  border: none;
  cursor: pointer;
  font-size: 16px;
  transition: all 0.3s;
  display: flex;
  align-items: center;
  white-space: nowrap;
}

/* 移动端菜单 */
.mobile-menu-btn {
  background: none;
  border: none;
  color: white;
  font-size: 1.5rem;
  cursor: pointer;
  display: none;
}

.mobile-menu-open {
  position: fixed;
  top: 60px;
  left: 0;
  right: 0;
  background: rgba(29, 33, 41, 0.95);
  backdrop-filter: blur(10px);
  padding: 15px 20px;
  z-index: 100;
  width: 100%;
  box-sizing: border-box;
}

.mobile-menu-open .nav-buttons {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-between;
  gap: 10px;
}

.mobile-menu-open .mode-btn,
.mobile-menu-open .logout-btn {
  flex: 1;
  min-width: calc(50% - 5px);
  text-align: center;
  justify-content: center;
}

/* 统计卡片区域 */
.stats-cards {
  display: flex;
  margin-top: 85px;
  padding: 0 40px;
  gap: 20px;
  margin-bottom: 25px;
  width: 100%;
  box-sizing: border-box;
}

.stat-card {
  flex: 1;
  border-radius: 16px;
  padding: 20px;
  transition: all 0.3s;
  min-width: 0;
}

/* 主要内容区域 */
.task-main {
  position: relative;
  padding: 0 20px 20px;
  width: 100%;
  box-sizing: border-box;
}

.main-content {
  border-radius: 16px;
  min-height: calc(100vh - 240px);
  padding: 20px;
  width: 100%;
  box-sizing: border-box;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .mobile-menu-btn {
    display: block;
  }

  .nav-buttons {
    display: none;
  }

  .stats-cards {
    flex-direction: column;
  }

  .stat-card {
    width: 100%;
  }

  .mobile-menu-open .nav-buttons {
    display: flex;
  }

  .task-logo {
    max-width: 150px;
  }
}

@media (max-width: 480px) {
  .mobile-menu-open .mode-btn,
  .mobile-menu-open .logout-btn {
    min-width: 100%;
  }

  .header-content {
    padding: 0 10px;
  }
}

/* 自定义工具类 */
.glass-effect {
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(10px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
}

.glass-effect-dark {
  background: rgba(29, 33, 41, 0.8);
  backdrop-filter: blur(10px);
}

.task-card-hover {
  transition: all 0.3s ease;
}

.task-card-hover:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgba(22, 93, 255, 0.15);
}

/* 自定义颜色 */
.bg-primary {
  background-color: #165DFF;
}

.bg-primary-light {
  background-color: #E8F3FF;
  border-radius: 15px;
  
}

.bg-success {
  background-color: #00B42A;
}

.bg-warning {
  background-color: #FF7D00;
}

.bg-danger {
  background-color: #F53F3F;
}

.bg-dark {
  background-color: #1D2129;
}

.bg-light {
  background-color: #F2F3F5;
}

.bg-muted {
  background-color: #86909C;
}

.text-primary {
  color: #165DFF;
}

.text-success {
  color: #00B42A;
}

.text-warning {
  color: #FF7D00;
}

.text-danger {
  color: #F53F3F;
}

.text-dark {
  color: #1D2129;
}

.text-light {
  color: #F2F3F5;
}

.text-muted {
  color: #86909C;
}

.border-primary {
  border-color: #165DFF;
}

.border-success {
  border-color: #00B42A;
}

.border-warning {
  border-color: #FF7D00;
}

.border-danger {
  border-color: #F53F3F;
}

.border-dark {
  border-color: #1D2129;
}

.border-light {
  border-color: #F2F3F5;
}

.border-muted {
  border-color: #86909C;
}

.transition-all {
  transition: all 0.3s ease;
}
</style>