<template>
  <div>
    <!-- 顶部筛选栏 -->
    <div class="filter-tabs">
      <div class="tab active">全部</div>
      <div class="tab">今日</div>
      <div class="tab">本周</div>
      <div class="tab">已完成</div>
      <div class="tab">未完成</div>
      <div class="tab">优先级</div>
    </div>

    <!-- 任务列表 -->
    <div class="task-list">
      <!-- 任务项1 -->
      <div class="task-item">
        <div class="task-content">
          <h3 class="task-title">产品需求文档更新</h3>
          <p class="task-desc">更新产品需求文档，增加新功能模块说明</p>
          <div class="task-meta">
            <span class="meta-item">
              <i class="far fa-clock"></i> 今天 15:30
            </span>
            <span class="meta-item">
              <i class="fas fa-tag"></i> 产品
            </span>
          </div>
        </div>
        <div class="task-status in-progress">进行中</div>
      </div>

      <!-- 任务项2 -->
      <div class="task-item">
        <div class="task-content">
          <h3 class="task-title">与客户视频会议</h3>
          <p class="task-desc">讨论项目进度和下阶段计划</p>
          <div class="task-meta">
            <span class="meta-item">
              <i class="far fa-clock"></i> 明天 10:00
            </span>
            <span class="meta-item">
              <i class="fas fa-tag"></i> 会议
            </span>
          </div>
        </div>
        <div class="task-status pending">待办</div>
      </div>

      <!-- 任务项3 -->
      <div class="task-item">
        <div class="task-content">
          <h3 class="task-title">前端页面设计</h3>
          <p class="task-desc">完成首页和详情页的UI设计</p>
          <div class="task-meta">
            <span class="meta-item">
              <i class="far fa-clock"></i> 昨天 18:45
            </span>
            <span class="meta-item">
              <i class="fas fa-tag"></i> 设计
            </span>
          </div>
        </div>
        <div class="task-status completed">已完成</div>
      </div>
    </div>

    <!-- 分页信息 -->
    <div class="pagination-info">
      显示 1-3 条，共 12 条
    </div>
  </div>
</template>

<script>
export default {
  name: 'TaskList'
}
</script>

<style scoped>
.task-container {
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
  justify-content: space-between;
  margin-bottom: 20px;
}

.new-task-btn {
  background-color: #1890ff;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.new-task-btn i {
  margin-right: 5px;
}

.search-box {
  display: flex;
  align-items: center;
  background-color: #f5f5f5;
  padding: 8px 12px;
  border-radius: 4px;
  width: 200px;
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
  align-items: center;
  padding: 15px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  margin-bottom: 10px;
}

.task-content {
  flex: 1;
}

.task-title {
  font-size: 16px;
  font-weight: 600;
  color: #333;
  margin: 0 0 8px 0;
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
}

.task-meta i {
  margin-right: 5px;
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

.pagination-info {
  text-align: center;
  font-size: 14px;
  color: #999;
  margin-top: 20px;
}
</style>