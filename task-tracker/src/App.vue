<script setup>
import { ref, computed } from 'vue'

const newTaskTitle = ref('')
const newTaskDescription = ref('')
const newTaskCategory = ref('Work')
const selectedFilter = ref('all')

const tasks = ref([
  {
    id: 1,
    title: 'Complete project proposal',
    description: 'Write and submit the Q4 project proposal',
    category: 'Work',
    status: 'active',
    createdAt: new Date()
  },
  {
    id: 2,
    title: 'Buy groceries',
    description: 'Get milk, bread, and eggs from the store',
    category: 'Personal',
    status: 'active',
    createdAt: new Date()
  },
  {
    id: 3,
    title: 'Team meeting',
    description: 'Discuss sprint planning for next week',
    category: 'Work',
    status: 'completed',
    createdAt: new Date()
  },
  {
    id: 4,
    title: 'Read book',
    description: 'Finish reading "Atomic Habits"',
    category: 'Personal',
    status: 'active',
    createdAt: new Date()
  },
  {
    id: 5,
    title: 'Volunteer event',
    description: 'Help at the community center',
    category: 'Other',
    status: 'completed',
    createdAt: new Date()
  }
])

const addTask = () => {
  if (!newTaskTitle.value.trim()) {
    return
  }
  
  const maxId = tasks.value.length > 0 
    ? Math.max(...tasks.value.map(task => task.id)) 
    : 0
  
  tasks.value.push({
    id: maxId + 1,
    title: newTaskTitle.value,
    description: newTaskDescription.value,
    category: newTaskCategory.value,
    status: 'active',
    createdAt: new Date()
  })
  
  // Reset form
  newTaskTitle.value = ''
  newTaskDescription.value = ''
  newTaskCategory.value = 'Work'
}

const toggleTaskStatus = (taskId) => {
  const task = tasks.value.find(t => t.id === taskId)
  if (task) {
    task.status = task.status === 'active' ? 'completed' : 'active'
  }
}

const deleteTask = (taskId) => {
  const index = tasks.value.findIndex(t => t.id === taskId)
  if (index !== -1) {
    tasks.value.splice(index, 1)
  }
}

const filteredTasks = computed(() => {
  if (selectedFilter.value === 'active') {
    return tasks.value.filter(task => task.status === 'active')
  } else if (selectedFilter.value === 'completed') {
    return tasks.value.filter(task => task.status === 'completed')
  }
  return tasks.value
})

const totalTasks = computed(() => tasks.value.length)

const activeTasks = computed(() => 
  tasks.value.filter(task => task.status === 'active').length
)

const completedTasks = computed(() => 
  tasks.value.filter(task => task.status === 'completed').length
)
</script>

<template>
  <div class="app">
    <h1>📝 Task Tracker</h1>
    
    <!-- Add Task Panel -->
    <div class="add-task-panel">
      <h2>➕ Add New Task</h2>
      <form @submit.prevent="addTask">
        <div class="form-group">
          <label for="task-title">Title</label>
          <input 
            id="task-title"
            v-model="newTaskTitle" 
            type="text" 
            placeholder="Enter task title..."
            required
          >
        </div>
        
        <div class="form-group">
          <label for="task-description">Description</label>
          <textarea 
            id="task-description"
            v-model="newTaskDescription" 
            placeholder="Enter task description..."
            rows="3"
          ></textarea>
        </div>
        
        <div class="form-group">
          <label for="task-category">Category</label>
          <select id="task-category" v-model="newTaskCategory">
            <option value="Work">Work</option>
            <option value="Personal">Personal</option>
            <option value="Other">Other</option>
          </select>
        </div>
        
        <button type="submit" class="add-btn">Add Task</button>
      </form>
    </div>
    
    <!-- Filter Panel -->
    <div class="filter-panel">
      <h3>Filter Tasks</h3>
      <div class="filter-buttons">
        <button 
          :class="{ active: selectedFilter === 'all' }" 
          @click="selectedFilter = 'all'"
        >
          All
        </button>
        <button 
          :class="{ active: selectedFilter === 'active' }" 
          @click="selectedFilter = 'active'"
        >
          Active
        </button>
        <button 
          :class="{ active: selectedFilter === 'completed' }" 
          @click="selectedFilter = 'completed'"
        >
          Completed
        </button>
      </div>
    </div>
    
    <!-- Statistics Panel -->
    <div class="stats-panel">
      <div class="stat">
        <span class="stat-label">Total Tasks</span>
        <span class="stat-value">{{ totalTasks }}</span>
      </div>
      <div class="stat">
        <span class="stat-label">Active</span>
        <span class="stat-value active">{{ activeTasks }}</span>
      </div>
      <div class="stat">
        <span class="stat-label">Completed</span>
        <span class="stat-value completed">{{ completedTasks }}</span>
      </div>
    </div>
    
    <div class="task-list">
      <div 
        v-for="task in filteredTasks" 
        :key="task.id" 
        class="task-card"
        :class="{ completed: task.status === 'completed' }"
      >
        <div class="task-header">
          <h3>{{ task.title }}</h3>
          <button class="delete-btn" @click="deleteTask(task.id)" title="Delete task">✕</button>
        </div>
        
        <p class="task-description">{{ task.description }}</p>
        
        <div class="task-footer">
          <span class="status-badge" :class="task.status" @click="toggleTaskStatus(task.id)">
            {{ task.status === 'completed' ? '✓ Completed' : '○ Active' }}
          </span>
          <span class="category-badge" :class="task.category.toLowerCase()">
            {{ task.category }}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.app {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

h1 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 2rem;
}

.add-task-panel {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 12px;
  padding: 2rem;
  margin-bottom: 2rem;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.add-task-panel h2 {
  margin: 0 0 1.5rem 0;
  color: white;
  font-size: 1.5rem;
}

.add-task-panel form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.form-group label {
  color: white;
  font-weight: 600;
  font-size: 0.9rem;
}

.form-group input,
.form-group textarea,
.form-group select {
  padding: 0.75rem;
  border: 2px solid rgba(255, 255, 255, 0.3);
  border-radius: 6px;
  font-size: 1rem;
  background: white;
  color: #2c3e50;
  transition: border-color 0.2s ease;
}

.form-group input:focus,
.form-group textarea:focus,
.form-group select:focus {
  outline: none;
  border-color: white;
}

.form-group textarea {
  resize: vertical;
  font-family: inherit;
}

.add-btn {
  padding: 0.875rem 2rem;
  background: white;
  color: #667eea;
  border: none;
  border-radius: 6px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  margin-top: 0.5rem;
}

.add-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.add-btn:active {
  transform: translateY(0);
}

.filter-panel {
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  margin-bottom: 2rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.filter-panel h3 {
  margin: 0 0 1rem 0;
  color: #2c3e50;
  font-size: 1.1rem;
}

.filter-buttons {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
}

.filter-buttons button {
  padding: 0.625rem 1.5rem;
  background: #f0f0f0;
  color: #2c3e50;
  border: 2px solid transparent;
  border-radius: 6px;
  font-size: 0.95rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
}

.filter-buttons button:hover {
  background: #e0e0e0;
  transform: translateY(-2px);
}

.filter-buttons button.active {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-color: #667eea;
}

.stats-panel {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 1.5rem;
  margin-bottom: 2rem;
}

.stat {
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.stat-label {
  display: block;
  color: #666;
  font-size: 0.85rem;
  font-weight: 600;
  text-transform: uppercase;
  margin-bottom: 0.5rem;
  letter-spacing: 0.5px;
}

.stat-value {
  display: block;
  color: #2c3e50;
  font-size: 2rem;
  font-weight: bold;
}

.stat-value.active {
  color: #2e7d32;
}

.stat-value.completed {
  color: #616161;
}

.task-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1.5rem;
}

.task-card {
  background: white;
  border-radius: 8px;
  padding: 1.5rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.task-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.task-card.completed {
  opacity: 0.7;
  background: #f8f9fa;
}

.task-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 1rem;
  gap: 1rem;
}

.task-header h3 {
  margin: 0;
  color: #2c3e50;
  font-size: 1.25rem;
}

.task-card.completed h3 {
  text-decoration: line-through;
}

.category-badge {
  padding: 0.25rem 0.75rem;
  border-radius: 12px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  white-space: nowrap;
}

.category-badge.work {
  background: #e3f2fd;
  color: #1976d2;
}

.category-badge.personal {
  background: #f3e5f5;
  color: #7b1fa2;
}

.category-badge.other {
  background: #fff3e0;
  color: #f57c00;
}

.task-description {
  color: #555;
  line-height: 1.6;
  margin: 0 0 1rem 0;
}

.task-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
}

.status-badge {
  padding: 0.5rem 1rem;
  border-radius: 6px;
  font-size: 0.875rem;
  font-weight: 500;
  transition: transform 0.2s ease;
  cursor: pointer;
}

.status-badge:hover {
  transform: scale(1.05);
}

.delete-btn {
  background: #ff6b6b;
  color: white;
  border: none;
  border-radius: 4px;
  width: 24px;
  height: 24px;
  font-size: 0.85rem;
  font-weight: bold;
  cursor: pointer;
  transition: background 0.2s ease, transform 0.2s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0;
  margin-left: auto;
}

.delete-btn:hover {
  background: #ff5252;
  transform: scale(1.1);
}

.delete-btn:active {
  transform: scale(0.95);
}

.status-badge.active {
  background: #e8f5e9;
  color: #2e7d32;
}

.status-badge.completed {
  background: #e0e0e0;
  color: #616161;
}
</style>
