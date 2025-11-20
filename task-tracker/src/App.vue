<script setup>
import { ref } from 'vue'

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
</script>

<template>
  <div class="app">
    <h1>📝 Task Tracker</h1>
    
    <div class="task-list">
      <div 
        v-for="task in tasks" 
        :key="task.id" 
        class="task-card"
        :class="{ completed: task.status === 'completed' }"
      >
        <div class="task-header">
          <h3>{{ task.title }}</h3>
          <span class="category-badge" :class="task.category.toLowerCase()">
            {{ task.category }}
          </span>
        </div>
        
        <p class="task-description">{{ task.description }}</p>
        
        <div class="task-footer">
          <span class="status-badge" :class="task.status">
            {{ task.status === 'completed' ? '✓ Completed' : '○ Active' }}
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
  justify-content: flex-start;
}

.status-badge {
  padding: 0.5rem 1rem;
  border-radius: 6px;
  font-size: 0.875rem;
  font-weight: 500;
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
