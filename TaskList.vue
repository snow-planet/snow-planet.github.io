<template>
  <div class="optimized-task-container">
    <!-- 顶部筛选栏 -->
    <div class="filter-tabs">
      <div 
        class="tab" 
        :class="{ active: filterStatus === 'all' }"
        @click="filterStatus = 'all'; applyFilter()"
      >
        全部
      </div>
      <div 
        class="tab" 
        :class="{ active: filterStatus === 'pending' }"
        @click="filterStatus = 'pending'; applyFilter()"
      >
        未完成
      </div>
      <div 
        class="tab" 
        :class="{ active: filterStatus === 'completed' }"
        @click="filterStatus = 'completed'; applyFilter()"
      >
        已完成
      </div>
    </div>

    <!-- 搜索和过滤区域 -->
    <div class="task-actions">
      <div class="search-box">
        <i class="fas fa-search"></i>
        <input 
          v-model="nameFilter" 
          type="text" 
          placeholder="搜索任务..." 
          @input="applyFilter"
        >
      </div>
      <div class="search-box">
        <i class="fas fa-tag"></i>
        <input 
          v-model="tagFilter" 
          type="text" 
          placeholder="按标签查找" 
          @input="applyFilter"
        >
      </div>
    </div>

    <!-- 任务列表 -->
    <div class="task-list">
      <!-- 任务项1 -->
      <div class="task-item" v-if="showTask('1')">
        <div class="task-content">
          <div class="task-title-row">
            <input 
              type="checkbox" 
              class="task-checkbox"
              :checked="false"
              @change="toggleTask('1')"
            >
            <h3 class="task-title">产品需求文档更新</h3>
            <div class="task-status in-progress">进行中</div>
          </div>
          <p class="task-desc">更新产品需求文档，增加新功能模块说明</p>
          <div class="task-meta">
            <span class="meta-item">
              <i class="far fa-clock"></i> 今天 15:30
            </span>
            <span class="meta-item">
              <i class="fas fa-tag"></i> 产品
            </span>
          </div>
          <div class="task-tags">
            <span class="tag">重要</span>
            <span class="tag">紧急</span>
          </div>
        </div>
        <div class="task-actions">
          <button class="action-btn delete-btn" @click="deleteTask('1')">
            <i class="fas fa-trash"></i>
          </button>
          <router-link 
            :to="{ name: 'TaskDetail', params: { id: '1' } }" 
            class="action-btn detail-btn"
          >
            <i class="fas fa-eye"></i> 详情
          </router-link>
        </div>
      </div>

      <!-- 任务项2 -->
      <div class="task-item" v-if="showTask('2')">
        <div class="task-content">
          <div class="task-title-row">
            <input 
              type="checkbox" 
              class="task-checkbox"
              :checked="false"
              @change="toggleTask('2')"
            >
            <h3 class="task-title">与客户视频会议</h3>
            <div class="task-status pending">待办</div>
          </div>
          <p class="task-desc">讨论项目进度和下阶段计划</p>
          <div class="task-meta">
            <span class="meta-item">
              <i class="far fa-clock"></i> 明天 10:00
            </span>
            <span class="meta-item">
              <i class="fas fa-tag"></i> 会议
            </span>
          </div>
          <div class="task-tags">
            <span class="tag">会议</span>
            <span class="tag">客户</span>
          </div>
        </div>
        <div class="task-actions">
          <button class="action-btn delete-btn" @click="deleteTask('2')">
            <i class="fas fa-trash"></i>
          </button>
          <router-link 
            :to="{ name: 'TaskDetail', params: { id: '2' } }" 
            class="action-btn detail-btn"
          >
            <i class="fas fa-eye"></i> 详情
          </router-link>
        </div>
      </div>

      <!-- 任务项3 -->
      <div class="task-item" v-if="showTask('3')">
        <div class="task-content">
          <div class="task-title-row">
            <input 
              type="checkbox" 
              class="task-checkbox"
              :checked="true"
              @change="toggleTask('3')"
            >
            <h3 class="task-title">前端页面设计</h3>
            <div class="task-status completed">已完成</div>
          </div>
          <p class="task-desc">完成首页和详情页的UI设计</p>
          <div class="task-meta">
            <span class="meta-item">
              <i class="far fa-clock"></i> 昨天 18:45
            </span>
            <span class="meta-item">
              <i class="fas fa-tag"></i> 设计
            </span>
          </div>
          <div class="task-tags">
            <span class="tag">UI</span>
            <span class="tag">设计稿</span>
          </div>
        </div>
        <div class="task-actions">
          <button class="action-btn delete-btn" @click="deleteTask('3')">
            <i class="fas fa-trash"></i>
          </button>
          <router-link 
            :to="{ name: 'TaskDetail', params: { id: '3' } }" 
            class="action-btn detail-btn"
          >
            <i class="fas fa-eye"></i> 详情
          </router-link>
        </div>
      </div>
    </div>

    <!-- 分页信息 -->
    <div class="pagination-info">
      显示 {{ visibleTaskCount }}-3 条，共 3 条
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const filterStatus = ref('all');
const tagFilter = ref('');
const nameFilter = ref('');

// 使用静态模拟数据
const tasks = ref([
  {
    id: '1',
    title: '产品需求文档更新',
    desc: '更新产品需求文档，增加新功能模块说明',
    time: '今天 15:30',
    category: '产品',
    status: 'in-progress',
    completed: false,
    tags: ['重要', '紧急']
  },
  {
    id: '2',
    title: '与客户视频会议',
    desc: '讨论项目进度和下阶段计划',
    time: '明天 10:00',
    category: '会议',
    status: 'pending',
    completed: false,
    tags: ['会议', '客户']
  },
  {
    id: '3',
    title: '前端页面设计',
    desc: '完成首页和详情页的UI设计',
    time: '昨天 18:45',
    category: '设计',
    status: 'completed',
    completed: true,
    tags: ['UI', '设计稿']
  }
]);

const visibleTaskCount = computed(() => {
  return tasks.value.filter(task => showTask(task.id)).length;
});

const showTask = (id) => {
  const task = tasks.value.find(t => t.id === id);
  if (!task) return false;
  
  // 状态过滤
  if (filterStatus.value === 'completed' && !task.completed) return false;
  if (filterStatus.value === 'pending' && task.completed) return false;
  
  // 名称过滤
  if (nameFilter.value && !task.title.includes(nameFilter.value)) return false;
  
  // 标签过滤
  if (tagFilter.value && !task.tags.some(tag => tag.includes(tagFilter.value))) return false;
  
  return true;
};

const toggleTask = (id) => {
  const task = tasks.value.find(t => t.id === id);
  if (task) {
    task.completed = !task.completed;
    task.status = task.completed ? 'completed' : 'pending';
  }
};

const deleteTask = (id) => {
  tasks.value = tasks.value.filter(task => task.id !== id);
};

const applyFilter = () => {
  // 过滤逻辑已在 showTask 方法中实现
};
</script>

<style scoped>
.optimized-task-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
}

.filter-tabs {
  display: flex;
  gap: 15px;
  margin-bottom: 20px;
  border-bottom: 1px solid #eaeaea;
  padding-bottom: 10px;
}

.tab {
  padding: 5px 10px;
  cursor: pointer;
  color: #666;
  font-size: 14px;
}

.tab.active {
  color: #1890ff;
  border-bottom: 2px solid #1890ff;
}

.task-actions {
  display: flex;
  gap: 15px;
  margin-bottom: 20px;
}

.search-box {
  display: flex;
  align-items: center;
  background-color: #f5f5f5;
  padding: 8px 12px;
  border-radius: 4px;
  flex: 1;
}

.search-box i {
  color: #999;
  margin-right: 8px;
}

.search-box input {
  border: none;
  background: transparent;
  outline: none;
  width: 100%;
}

.task-list {
  margin-bottom: 20px;
}

.task-item {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  padding: 15px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  margin-bottom: 10px;
}

.task-content {
  flex: 1;
}

.task-title-row {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 8px;
}

.task-checkbox {
  width: 16px;
  height: 16px;
  cursor: pointer;
}

.task-title {
  font-size: 16px;
  font-weight: 600;
  color: #333;
  margin: 0;
  flex: 1;
}

.task-desc {
  font-size: 14px;
  color: #666;
  margin: 0 0 12px 0;
}

.task-meta {
  display: flex;
  gap: 15px;
  font-size: 12px;
  color: #999;
  margin-bottom: 10px;
}

.task-meta i {
  margin-right: 5px;
}

.task-tags {
  display: flex;
  gap: 8px;
}

.tag {
  font-size: 12px;
  padding: 2px 8px;
  background-color: #f0f0f0;
  border-radius: 10px;
  color: #666;
}

.task-status {
  font-size: 12px;
  padding: 4px 10px;
  border-radius: 10px;
}

.task-status.in-progress {
  background-color: #e6f7ff;
  color: #1890ff;
  border: 1px solid #91d5ff;
}

.task-status.pending {
  background-color: #fff7e6;
  color: #fa8c16;
  border: 1px solid #ffd591;
}

.task-status.completed {
  background-color: #f6ffed;
  color: #52c41a;
  border: 1px solid #b7eb8f;
}

.task-actions {
  display: flex;
  gap: 8px;
}

.action-btn {
  padding: 6px 12px;
  font-size: 14px;
  border-radius: 4px;
  border: 1px solid #d9d9d9;
  background-color: white;
  color: #666;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 5px;
}

.action-btn i {
  font-size: 12px;
}

.delete-btn {
  color: #ff4d4f;
  border-color: #ffa39e;
}

.delete-btn:hover {
  background-color: #fff1f0;
}

.detail-btn {
  color: #1890ff;
  border-color: #91d5ff;
}

.detail-btn:hover {
  background-color: #e6f7ff;
}

.pagination-info {
  text-align: center;
  font-size: 14px;
  color: #999;
  margin-top: 20px;
}
</style>