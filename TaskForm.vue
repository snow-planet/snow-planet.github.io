<template>
    <form @submit.prevent="addTask" class="enhanced-task-form">
      <!-- 任务标题 -->
      <div class="input-group">
        <input v-model="newTask.title" type="text" placeholder="任务标题" required class="task-input">
      </div>
      
      <!-- 重要性等级 -->
      <div class="priority-section">
        <label>重要性：</label>
        <div class="priority-options">
          <button 
            v-for="level in priorityLevels" 
            :key="level.value"
            type="button"
            :class="['priority-btn', { active: newTask.priority === level.value }]"
            @click="newTask.priority = level.value"
          >
            <span class="priority-icon" :style="{ color: level.color }">⬤</span>
            {{ level.label }}
          </button>
        </div>
      </div>
      
      <!-- 任务标签 -->
      <div class="tags-section">
        <label>标签：</label>
        <div class="tags-input">
          <input 
            v-model="tagInput" 
            type="text" 
            placeholder="输入标签后按Enter添加"
            @keydown.enter.prevent="addTag"
            class="tag-input-field"
          >
          <div class="tags-list">
            <span v-for="(tag, index) in newTask.tags" :key="index" class="tag-item">
              {{ tag }}
              <span @click="removeTag(index)" class="tag-remove">×</span>
            </span>
          </div>
        </div>
      </div>
      
      <!-- 详细描述 -->
      <div class="description-section">
        <label>详细描述：</label>
        <textarea 
          v-model="newTask.description" 
          placeholder="输入任务详细描述..."
          class="description-textarea"
        ></textarea>
      </div>
      
      <!-- 文件上传 -->
      <div class="file-section">
        <label>附件：</label>
        <div class="file-upload">
          <input 
            type="file" 
            ref="fileInput"
            @change="handleFileUpload"
            multiple
            class="file-input"
          >
          <button type="button" @click="triggerFileUpload" class="upload-btn">
            <i class="fas fa-paperclip"></i> 选择文件
          </button>
          <div v-if="newTask.files.length > 0" class="file-list">
            <div v-for="(file, index) in newTask.files" :key="index" class="file-item">
              <span class="file-name">{{ file.name }}</span>
              <span @click="removeFile(index)" class="file-remove">×</span>
            </div>
          </div>
        </div>
      </div>
      
      <!-- 提交按钮 -->
      <button type="submit" class="submit-btn">添加任务</button>
    </form>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import { useTaskStore } from '../../store';
  
  const taskStore = useTaskStore();
  const fileInput = ref(null);
  
  const priorityLevels = [
    { value: 1, label: '低', color: '#28a745' },
    { value: 2, label: '中', color: '#ffc107' },
    { value: 3, label: '高', color: '#dc3545' }
  ];
  
  const newTask = ref({
    title: '',
    priority: 2, // 默认为中等优先级
    description: '',
    tags: [],
    files: []
  });
  
  const tagInput = ref('');
  
  const addTask = () => {
    if (newTask.value.title.trim()) {
      taskStore.addTask({
        id: Date.now(),
        ...newTask.value,
        completed: false,
        createdAt: new Date()
      });
      resetForm();
    }
  };
  
  const addTag = () => {
    if (tagInput.value.trim() && !newTask.value.tags.includes(tagInput.value.trim())) {
      newTask.value.tags.push(tagInput.value.trim());
      tagInput.value = '';
    }
  };
  
  const removeTag = (index) => {
    newTask.value.tags.splice(index, 1);
  };
  
  const triggerFileUpload = () => {
    fileInput.value.click();
  };
  
  const handleFileUpload = (event) => {
    const files = Array.from(event.target.files);
    newTask.value.files = [...newTask.value.files, ...files];
    event.target.value = ''; // 重置文件输入
  };
  
  const removeFile = (index) => {
    newTask.value.files.splice(index, 1);
  };
  
  const resetForm = () => {
    newTask.value = {
      title: '',
      priority: 2,
      description: '',
      tags: [],
      files: []
    };
    tagInput.value = '';
  };
  </script>
  
  <style scoped>
  .enhanced-task-form {
    display: flex;
    flex-direction: column;
    gap: 20px;
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
    background: white;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  
  .input-group {
    display: flex;
    width: 100%;
  }
  
  .task-input {
    flex-grow: 1;
    padding: 12px;
    border: 1px solid #ddd;
    border-radius: 6px;
    font-size: 16px;
    outline: none;
    transition: border-color 0.3s;
  }
  
  .task-input:focus {
    border-color: #007bff;
  }
  
  .priority-section,
  .tags-section,
  .description-section,
  .file-section {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }
  
  label {
    font-weight: 500;
    color: #495057;
  }
  
  .priority-options {
    display: flex;
    gap: 10px;
  }
  
  .priority-btn {
    display: flex;
    align-items: center;
    gap: 5px;
    padding: 8px 12px;
    background: #f8f9fa;
    border: 1px solid #ddd;
    border-radius: 20px;
    cursor: pointer;
    transition: all 0.3s;
  }
  
  .priority-btn.active {
    background: #e9ecef;
    border-color: #adb5bd;
  }
  
  .priority-icon {
    font-size: 12px;
  }
  
  .tags-input {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }
  
  .tag-input-field {
    padding: 8px 12px;
    border: 1px solid #ddd;
    border-radius: 4px;
    outline: none;
    transition: border-color 0.3s;
  }
  
  .tag-input-field:focus {
    border-color: #007bff;
  }
  
  .tags-list {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
  }
  
  .tag-item {
    display: inline-flex;
    align-items: center;
    padding: 4px 8px;
    background: #e9ecef;
    border-radius: 4px;
    font-size: 14px;
  }
  
  .tag-remove {
    margin-left: 5px;
    cursor: pointer;
    color: #dc3545;
  }
  
  .description-textarea {
    width: 100%;
    min-height: 100px;
    padding: 12px;
    border: 1px solid #ddd;
    border-radius: 6px;
    resize: vertical;
    outline: none;
    transition: border-color 0.3s;
  }
  
  .description-textarea:focus {
    border-color: #007bff;
  }
  
  .file-upload {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }
  
  .file-input {
    display: none;
  }
  
  .upload-btn {
    align-self: flex-start;
    padding: 8px 16px;
    background: #f8f9fa;
    border: 1px solid #ddd;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.3s;
  }
  
  .upload-btn:hover {
    background: #e9ecef;
  }
  
  .file-list {
    display: flex;
    flex-direction: column;
    gap: 5px;
  }
  
  .file-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 6px 10px;
    background: #f8f9fa;
    border-radius: 4px;
    font-size: 14px;
  }
  
  .file-name {
    flex-grow: 1;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  
  .file-remove {
    margin-left: 10px;
    cursor: pointer;
    color: #dc3545;
  }
  
  .submit-btn {
    padding: 12px;
    background: #007bff;
    color: white;
    border: none;
    border-radius: 6px;
    font-size: 16px;
    font-weight: 500;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .submit-btn:hover {
    background: #0056b3;
  }
  </style>