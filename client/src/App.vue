<template>
  <div class="app">
    <aside class="sidebar" :class="{ collapsed: isCollapsed }">
      <div class="sidebar-brand">
        <div class="brand-icon">
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
            <rect x="2" y="2" width="7" height="7" rx="1.5" fill="currentColor"/>
            <rect x="11" y="2" width="7" height="7" rx="1.5" fill="currentColor" opacity="0.7"/>
            <rect x="2" y="11" width="7" height="7" rx="1.5" fill="currentColor" opacity="0.7"/>
            <rect x="11" y="11" width="7" height="7" rx="1.5" fill="currentColor"/>
          </svg>
        </div>
        <div class="brand-text">
          <span class="brand-name">{{ t('nav.companyName') }}</span>
          <span class="brand-subtitle">{{ t('nav.subtitle') }}</span>
        </div>
        <button class="sidebar-toggle" @click="toggleSidebar" :title="isCollapsed ? 'Expand sidebar' : 'Collapse sidebar'">
          <svg width="14" height="14" viewBox="0 0 14 14" fill="none">
            <path :d="isCollapsed ? 'M5 2l5 5-5 5' : 'M9 2L4 7l5 5'" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
      </div>

      <nav class="sidebar-nav">
        <router-link to="/" class="nav-item" :class="{ active: $route.path === '/' }" :title="isCollapsed ? t('nav.overview') : ''">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <rect x="1" y="1" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
            <rect x="11" y="1" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
            <rect x="1" y="11" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
            <rect x="11" y="11" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <span v-show="!isCollapsed">{{ t('nav.overview') }}</span>
        </router-link>
        <router-link to="/inventory" class="nav-item" :class="{ active: $route.path === '/inventory' }" :title="isCollapsed ? t('nav.inventory') : ''">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <rect x="1" y="2" width="16" height="14" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <path d="M1 6h16M1 10h16" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <span v-show="!isCollapsed">{{ t('nav.inventory') }}</span>
        </router-link>
        <router-link to="/orders" class="nav-item" :class="{ active: $route.path === '/orders' }" :title="isCollapsed ? t('nav.orders') : ''">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <path d="M2 4h14l-1.5 9H3.5L2 4Z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
            <circle cx="6" cy="15.5" r="1" stroke="currentColor" stroke-width="1.5"/>
            <circle cx="12" cy="15.5" r="1" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <span v-show="!isCollapsed">{{ t('nav.orders') }}</span>
        </router-link>
        <router-link to="/spending" class="nav-item" :class="{ active: $route.path === '/spending' }" :title="isCollapsed ? t('nav.finance') : ''">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <circle cx="9" cy="9" r="7" stroke="currentColor" stroke-width="1.5"/>
            <path d="M9 4.5V6M9 12v1.5M6.5 7.5C6.5 6.7 7.2 6 9 6s2.5.9 2.5 2c0 2-5 2-5 4 0 1.1 1 2 2.5 2s2.5-.9 2.5-2" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span v-show="!isCollapsed">{{ t('nav.finance') }}</span>
        </router-link>
        <router-link to="/demand" class="nav-item" :class="{ active: $route.path === '/demand' }" :title="isCollapsed ? t('nav.demandForecast') : ''">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <polyline points="1,14 5,9 9,11 13,5 17,8" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M1 17h16" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span v-show="!isCollapsed">{{ t('nav.demandForecast') }}</span>
        </router-link>
        <router-link to="/reports" class="nav-item" :class="{ active: $route.path === '/reports' }" :title="isCollapsed ? 'Reports' : ''">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <rect x="2" y="1" width="14" height="16" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <path d="M5 6h8M5 9h8M5 12h5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span v-show="!isCollapsed">Reports</span>
        </router-link>
      </nav>

      <div class="sidebar-filters" v-show="!isCollapsed">
        <div class="sidebar-section-label">Filters</div>
        <FilterBar />
      </div>

      <div class="sidebar-footer">
        <template v-if="!isCollapsed">
          <LanguageSwitcher />
          <ProfileMenu
            @show-profile-details="showProfileDetails = true"
            @show-tasks="showTasks = true"
          />
        </template>
        <template v-else>
          <div class="collapsed-avatar" :title="'Profile'" @click="showProfileDetails = true">{{ getInitials(currentUser.name) }}</div>
        </template>
      </div>
    </aside>

    <main class="main-content">
      <router-view />
    </main>

    <ProfileDetailsModal
      :is-open="showProfileDetails"
      @close="showProfileDetails = false"
    />
    <TasksModal
      :is-open="showTasks"
      :tasks="tasks"
      @close="showTasks = false"
      @add-task="addTask"
      @delete-task="deleteTask"
      @toggle-task="toggleTask"
    />
  </div>
</template>

<script>
import { ref, onMounted, computed } from 'vue'
import { api } from './api'
import { useAuth } from './composables/useAuth'
import { useI18n } from './composables/useI18n'
import FilterBar from './components/FilterBar.vue'
import ProfileMenu from './components/ProfileMenu.vue'
import ProfileDetailsModal from './components/ProfileDetailsModal.vue'
import TasksModal from './components/TasksModal.vue'
import LanguageSwitcher from './components/LanguageSwitcher.vue'

export default {
  name: 'App',
  components: {
    FilterBar,
    ProfileMenu,
    ProfileDetailsModal,
    TasksModal,
    LanguageSwitcher
  },
  setup() {
    const { currentUser, getInitials } = useAuth()
    const { t } = useI18n()

    // Restore sidebar state from localStorage; collapse by default on narrow screens
    const isCollapsed = ref(
      localStorage.getItem('sidebar-collapsed') === 'true' ||
      window.innerWidth <= 900
    )

    const toggleSidebar = () => {
      isCollapsed.value = !isCollapsed.value
      localStorage.setItem('sidebar-collapsed', isCollapsed.value)
    }

    const showProfileDetails = ref(false)
    const showTasks = ref(false)
    const apiTasks = ref([])

    // Merge mock tasks from currentUser with API tasks
    const tasks = computed(() => {
      return [...currentUser.value.tasks, ...apiTasks.value]
    })

    const loadTasks = async () => {
      try {
        apiTasks.value = await api.getTasks()
      } catch (err) {
        console.error('Failed to load tasks:', err)
      }
    }

    const addTask = async (taskData) => {
      try {
        const newTask = await api.createTask(taskData)
        // Add new task to the beginning of the array
        apiTasks.value.unshift(newTask)
      } catch (err) {
        console.error('Failed to add task:', err)
      }
    }

    const deleteTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const isMockTask = currentUser.value.tasks.some(t => t.id === taskId)

        if (isMockTask) {
          // Remove from mock tasks
          const index = currentUser.value.tasks.findIndex(t => t.id === taskId)
          if (index !== -1) {
            currentUser.value.tasks.splice(index, 1)
          }
        } else {
          // Remove from API tasks
          await api.deleteTask(taskId)
          apiTasks.value = apiTasks.value.filter(t => t.id !== taskId)
        }
      } catch (err) {
        console.error('Failed to delete task:', err)
      }
    }

    const toggleTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const mockTask = currentUser.value.tasks.find(t => t.id === taskId)

        if (mockTask) {
          // Toggle mock task status
          mockTask.status = mockTask.status === 'pending' ? 'completed' : 'pending'
        } else {
          // Toggle API task
          const updatedTask = await api.toggleTask(taskId)
          const index = apiTasks.value.findIndex(t => t.id === taskId)
          if (index !== -1) {
            apiTasks.value[index] = updatedTask
          }
        }
      } catch (err) {
        console.error('Failed to toggle task:', err)
      }
    }

    const handleResize = () => {
      if (window.innerWidth <= 900) {
        isCollapsed.value = true
        localStorage.setItem('sidebar-collapsed', 'true')
      }
    }
    onMounted(() => {
      loadTasks()
      window.addEventListener('resize', handleResize)
    })

    return {
      t,
      isCollapsed,
      toggleSidebar,
      showProfileDetails,
      showTasks,
      tasks,
      addTask,
      deleteTask,
      toggleTask,
      currentUser,
      getInitials
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Syne:wght@600;700;800&family=DM+Mono:ital,wght@0,400;0,500;1,400&family=Geist:wght@400;500;600&display=swap');

:root {
  --sidebar-width: 240px;
  --bg: #0d1117;
  --surface: #161b22;
  --surface-2: #1c2128;
  --border: rgba(255,255,255,0.06);
  --border-strong: rgba(255,255,255,0.12);
  --accent: #6366f1;
  --accent-dim: rgba(99,102,241,0.12);
  --accent-glow: rgba(99,102,241,0.25);
  --text: #e6edf3;
  --text-2: #8b949e;
  --text-3: #484f58;
  --green: #3fb950;
  --yellow: #d29922;
  --red: #f85149;
  --blue: #58a6ff;
  --cyan: #79c0ff;
  --color-primary: #6366f1;
  --color-border: rgba(255,255,255,0.06);
  --color-text-primary: #e6edf3;
  --color-text-secondary: #8b949e;
  --color-surface: #161b22;
  --content-bg: #0d1117;
  --shadow-card: 0 1px 3px rgba(0,0,0,0.4);
  --shadow-card-hover: 0 4px 16px rgba(0,0,0,0.5);
}

* { margin: 0; padding: 0; box-sizing: border-box; }

body {
  font-family: 'Geist', system-ui, -apple-system, sans-serif;
  background: var(--bg);
  color: var(--text);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-size: 14px;
  line-height: 1.5;
}

.app {
  display: flex;
  flex-direction: row;
  min-height: 100vh;
}

/* ── Sidebar ──────────────────────────────────────────────────────────────── */
.sidebar {
  position: fixed;
  top: 0;
  left: 0;
  width: var(--sidebar-width);
  height: 100vh;
  background: var(--bg);
  border-right: 1px solid var(--border-strong);
  display: flex;
  flex-direction: column;
  z-index: 100;
  overflow: hidden;
}

.sidebar-brand {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 18px 16px 14px;
  border-bottom: 1px solid var(--border);
  flex-shrink: 0;
}

.brand-icon {
  width: 30px;
  height: 30px;
  background: var(--accent);
  border-radius: 6px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  flex-shrink: 0;
}

.brand-text { display: flex; flex-direction: column; min-width: 0; }

.brand-name {
  font-family: 'Syne', sans-serif;
  font-size: 0.8125rem;
  font-weight: 700;
  color: var(--text);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  letter-spacing: 0;
}

.brand-subtitle {
  font-size: 0.5625rem;
  color: var(--text-3);
  text-transform: uppercase;
  letter-spacing: 0.08em;
  margin-top: 1px;
}

.sidebar-nav {
  display: flex;
  flex-direction: column;
  gap: 1px;
  padding: 10px 8px;
  flex-shrink: 0;
}

.nav-item {
  display: flex;
  align-items: center;
  gap: 9px;
  padding: 6px 10px;
  border-radius: 5px;
  color: var(--text-2);
  text-decoration: none;
  font-size: 0.8125rem;
  font-weight: 500;
  transition: all 0.12s ease;
  position: relative;
  white-space: nowrap;
  font-family: 'Geist', sans-serif;
}

.nav-item:hover {
  background: rgba(255,255,255,0.04);
  color: var(--text);
}

.nav-item.active {
  background: var(--accent-dim);
  color: var(--accent);
}

/* Left accent bar for active nav item */
.nav-item.active::before {
  content: '';
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  width: 2px;
  height: 55%;
  background: var(--accent);
  border-radius: 0 2px 2px 0;
}

.nav-icon { flex-shrink: 0; opacity: 0.7; }
.nav-item.active .nav-icon { opacity: 1; }

.sidebar-filters {
  flex: 1;
  overflow-y: auto;
  /* min-height: 0 required — flex child must be allowed to shrink for overflow-y to activate */
  min-height: 0;
  padding: 0 8px 8px;
  border-top: 1px solid var(--border);
}

.sidebar-section-label {
  font-size: 0.5625rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--text-3);
  padding: 12px 8px 6px;
  font-family: 'Geist', sans-serif;
}

.sidebar-filters::-webkit-scrollbar { width: 3px; }
.sidebar-filters::-webkit-scrollbar-track { background: transparent; }
.sidebar-filters::-webkit-scrollbar-thumb { background: var(--border-strong); border-radius: 2px; }

.sidebar-footer {
  padding: 10px 8px;
  border-top: 1px solid var(--border);
  display: flex;
  flex-direction: column;
  gap: 4px;
  flex-shrink: 0;
}

/* ── Main content with animated top accent line ───────────────────────────── */
.main-content {
  flex: 1;
  margin-left: var(--sidebar-width);
  min-height: 100vh;
  padding: 1.5rem 2rem;
  background: var(--bg);
  position: relative;
}

.main-content::before {
  content: '';
  position: fixed;
  top: 0;
  left: var(--sidebar-width);
  right: 0;
  height: 2px;
  background: linear-gradient(90deg, #6366f1 0%, #22d3ee 50%, #6366f1 100%);
  background-size: 200% 100%;
  animation: pulse-line 4s linear infinite;
  z-index: 200;
}

@keyframes pulse-line {
  0% { background-position: 0% 0; }
  100% { background-position: 200% 0; }
}

/* ── Page headers ─────────────────────────────────────────────────────────── */
.page-header {
  margin-bottom: 1.5rem;
  padding-bottom: 1rem;
  border-bottom: 1px solid var(--border);
}

.page-header h2 {
  font-family: 'Syne', sans-serif;
  font-size: 1.375rem;
  font-weight: 700;
  color: var(--text);
  letter-spacing: -0.01em;
  margin-bottom: 0.25rem;
}

.page-header p {
  color: var(--text-2);
  font-size: 0.8125rem;
}

/* ── Stat cards ───────────────────────────────────────────────────────────── */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1px;
  margin-bottom: 1.5rem;
  background: var(--border);
  border: 1px solid var(--border);
  border-radius: 6px;
  overflow: hidden;
}

.stat-card {
  background: var(--surface);
  padding: 1rem 1.25rem;
  border-radius: 0;
  border: none;
  transition: background 0.15s ease;
  position: relative;
}

.stat-card:hover { background: var(--surface-2); }

.stat-card::before {
  content: '';
  display: block;
  width: 4px;
  height: 4px;
  border-radius: 1px;
  background: var(--text-3);
  margin-bottom: 0.625rem;
}

.stat-card.success::before { background: var(--green); }
.stat-card.warning::before { background: var(--yellow); }
.stat-card.danger::before  { background: var(--red); }
.stat-card.info::before    { background: var(--blue); }

.stat-label {
  color: var(--text-3);
  font-size: 0.6875rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.07em;
  margin-bottom: 0.375rem;
}

.stat-value {
  font-family: 'DM Mono', monospace;
  font-size: 1.625rem;
  font-weight: 500;
  color: var(--text);
  letter-spacing: -0.02em;
  line-height: 1.1;
}

.stat-card.warning .stat-value { color: var(--yellow); }
.stat-card.success .stat-value { color: var(--green); }
.stat-card.danger .stat-value  { color: var(--red); }
.stat-card.info .stat-value    { color: var(--blue); }

/* ── Cards ────────────────────────────────────────────────────────────────── */
.card {
  background: var(--surface);
  border-radius: 6px;
  padding: 0;
  border: 1px solid var(--border);
  margin-bottom: 1rem;
  overflow: hidden;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.875rem 1.25rem;
  border-bottom: 1px solid var(--border);
}

.card-title {
  font-family: 'Geist', sans-serif;
  font-size: 0.8125rem;
  font-weight: 600;
  color: var(--text);
  letter-spacing: 0;
}

/* ── Tables ───────────────────────────────────────────────────────────────── */
.table-container { overflow-x: auto; }

table { width: 100%; border-collapse: collapse; }

thead { border-bottom: 1px solid var(--border-strong); }

th {
  text-align: left;
  padding: 0.5rem 1rem;
  font-weight: 600;
  color: var(--text-3);
  font-size: 0.6875rem;
  text-transform: uppercase;
  letter-spacing: 0.07em;
  background: transparent;
  font-family: 'Geist', sans-serif;
}

td {
  padding: 0.5625rem 1rem;
  border-bottom: 1px solid var(--border);
  color: var(--text-2);
  font-size: 0.8125rem;
  font-family: 'Geist', sans-serif;
}

td:last-child { border-right: none; }

tbody tr { transition: background 0.1s ease; }
tbody tr:last-child td { border-bottom: none; }
tbody tr:hover { background: var(--surface-2); }
tbody tr:hover td { color: var(--text); }

/* Monospace for numeric columns — apply .mono class to td */
td.mono, .mono {
  font-family: 'DM Mono', monospace;
  font-size: 0.75rem;
  color: var(--text);
}

/* ── Badges ───────────────────────────────────────────────────────────────── */
.badge {
  display: inline-block;
  padding: 0.1875rem 0.5rem;
  border-radius: 3px;
  font-size: 0.625rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.07em;
  font-family: 'Geist', sans-serif;
}

.badge.success    { color: var(--green);  background: rgba(63,185,80,0.1);  border: 1px solid rgba(63,185,80,0.25); }
.badge.warning    { color: var(--yellow); background: rgba(210,153,34,0.1); border: 1px solid rgba(210,153,34,0.25); }
.badge.danger     { color: var(--red);    background: rgba(248,81,73,0.1);  border: 1px solid rgba(248,81,73,0.25); }
.badge.info       { color: var(--blue);   background: rgba(88,166,255,0.1); border: 1px solid rgba(88,166,255,0.25); }
.badge.increasing { color: var(--green);  background: rgba(63,185,80,0.1);  border: 1px solid rgba(63,185,80,0.25); }
.badge.decreasing { color: var(--red);    background: rgba(248,81,73,0.1);  border: 1px solid rgba(248,81,73,0.25); }
.badge.stable     { color: var(--blue);   background: rgba(88,166,255,0.1); border: 1px solid rgba(88,166,255,0.25); }
.badge.high       { color: var(--red);    background: rgba(248,81,73,0.1);  border: 1px solid rgba(248,81,73,0.25); }
.badge.medium     { color: var(--yellow); background: rgba(210,153,34,0.1); border: 1px solid rgba(210,153,34,0.25); }
.badge.low        { color: var(--blue);   background: rgba(88,166,255,0.1); border: 1px solid rgba(88,166,255,0.25); }

/* ── State utilities ──────────────────────────────────────────────────────── */
.loading {
  text-align: center;
  padding: 3rem;
  color: var(--text-3);
  font-size: 0.8125rem;
  font-family: 'DM Mono', monospace;
}

.error {
  background: rgba(248,81,73,0.08);
  border: 1px solid rgba(248,81,73,0.2);
  color: var(--red);
  padding: 0.875rem 1rem;
  border-radius: 5px;
  margin: 1rem 0;
  font-size: 0.8125rem;
}

/* ── Collapsible sidebar ──────────────────────────────────────────────────── */
.sidebar {
  transition: width 0.2s ease;
}

.sidebar.collapsed {
  width: 52px;
}

/* Toggle button */
.sidebar-toggle {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 22px;
  height: 22px;
  background: rgba(255,255,255,0.06);
  border: 1px solid var(--border-strong);
  border-radius: 4px;
  color: var(--text-3);
  cursor: pointer;
  transition: all 0.12s ease;
  flex-shrink: 0;
  margin-left: auto;
  padding: 0;
}

.sidebar-toggle:hover {
  background: rgba(255,255,255,0.1);
  color: var(--text);
}

/* In collapsed mode, center the brand icon and hide brand text */
.sidebar.collapsed .sidebar-brand {
  justify-content: center;
  padding: 14px 0;
  flex-direction: column;
  gap: 8px;
}

.sidebar.collapsed .brand-text {
  display: none;
}

.sidebar.collapsed .sidebar-toggle {
  margin-left: 0;
}

/* In collapsed mode, center nav icons */
.sidebar.collapsed .sidebar-nav {
  padding: 10px 0;
  align-items: center;
}

.sidebar.collapsed .nav-item {
  justify-content: center;
  padding: 8px 0;
  width: 36px;
  border-radius: 5px;
}

/* Hide the active indicator bar in collapsed mode to avoid misalignment */
.sidebar.collapsed .nav-item.active::before {
  display: none;
}

/* Collapsed footer: center the avatar */
.collapsed-avatar {
  width: 28px;
  height: 28px;
  border-radius: 50%;
  background: linear-gradient(135deg, #6366f1 0%, #4f46e5 100%);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  font-size: 0.625rem;
  cursor: pointer;
  margin: 0 auto;
  transition: opacity 0.15s;
}

.collapsed-avatar:hover { opacity: 0.8; }

.sidebar.collapsed .sidebar-footer {
  align-items: center;
  padding: 10px 0;
}

/* Main content width adjusts when sidebar collapses */
.app:has(.sidebar.collapsed) .main-content {
  margin-left: 52px;
}

.app:has(.sidebar.collapsed) .main-content::before {
  left: 52px;
}

/* Smooth main content transition */
.main-content {
  transition: margin-left 0.2s ease;
}
</style>
