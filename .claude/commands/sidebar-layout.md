---
description: Transform this Vue 3 app's top-navbar layout into a modern SaaS-style vertical sidebar layout with a dark fixed sidebar on the left and light scrollable content on the right.
---

Transform the Vue 3 app from a horizontal top-nav layout to a fixed vertical sidebar layout by modifying exactly these files in order. Do NOT modify any files in src/views/, src/composables/, src/api.js, src/locales/, src/main.js, or any modal components.

## File 1: client/src/App.vue

### Template block — replace entirely

Replace everything between `<template>` and `</template>` with:

```html
<template>
  <div class="app">
    <aside class="sidebar">
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
      </div>

      <nav class="sidebar-nav">
        <router-link to="/" class="nav-item" :class="{ active: $route.path === '/' }">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <rect x="1" y="1" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
            <rect x="11" y="1" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
            <rect x="1" y="11" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
            <rect x="11" y="11" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <span>{{ t('nav.overview') }}</span>
        </router-link>
        <router-link to="/inventory" class="nav-item" :class="{ active: $route.path === '/inventory' }">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <rect x="1" y="2" width="16" height="14" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <path d="M1 6h16M1 10h16" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <span>{{ t('nav.inventory') }}</span>
        </router-link>
        <router-link to="/orders" class="nav-item" :class="{ active: $route.path === '/orders' }">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <path d="M2 4h14l-1.5 9H3.5L2 4Z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
            <circle cx="6" cy="15.5" r="1" stroke="currentColor" stroke-width="1.5"/>
            <circle cx="12" cy="15.5" r="1" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <span>{{ t('nav.orders') }}</span>
        </router-link>
        <router-link to="/spending" class="nav-item" :class="{ active: $route.path === '/spending' }">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <circle cx="9" cy="9" r="7" stroke="currentColor" stroke-width="1.5"/>
            <path d="M9 4.5V6M9 12v1.5M6.5 7.5C6.5 6.7 7.2 6 9 6s2.5.9 2.5 2c0 2-5 2-5 4 0 1.1 1 2 2.5 2s2.5-.9 2.5-2" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span>{{ t('nav.finance') }}</span>
        </router-link>
        <router-link to="/demand" class="nav-item" :class="{ active: $route.path === '/demand' }">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <polyline points="1,14 5,9 9,11 13,5 17,8" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M1 17h16" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span>{{ t('nav.demandForecast') }}</span>
        </router-link>
        <router-link to="/reports" class="nav-item" :class="{ active: $route.path === '/reports' }">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <rect x="2" y="1" width="14" height="16" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <path d="M5 6h8M5 9h8M5 12h5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span>Reports</span>
        </router-link>
      </nav>

      <div class="sidebar-filters">
        <div class="sidebar-section-label">Filters</div>
        <FilterBar />
      </div>

      <div class="sidebar-footer">
        <LanguageSwitcher />
        <ProfileMenu
          @show-profile-details="showProfileDetails = true"
          @show-tasks="showTasks = true"
        />
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
```

### Script block — DO NOT CHANGE

Leave the entire `<script>` block exactly as-is.

### Style block — replace entirely

Replace everything between `<style>` and `</style>` with:

```css
:root {
  --sidebar-width: 240px;
  --sidebar-bg: #0f172a;
  --sidebar-border: rgba(255, 255, 255, 0.06);
  --sidebar-text: #94a3b8;
  --sidebar-text-hover: #f1f5f9;
  --sidebar-active-bg: rgba(37, 99, 235, 0.18);
  --sidebar-active-text: #60a5fa;
  --sidebar-active-indicator: #2563eb;
  --sidebar-section-label: #475569;
  --color-primary: #2563eb;
  --color-border: #e2e8f0;
  --color-text-primary: #0f172a;
  --color-text-secondary: #64748b;
  --color-surface: #ffffff;
  --content-bg: #f8fafc;
  --shadow-card: 0 1px 3px rgba(0, 0, 0, 0.06), 0 1px 2px rgba(0, 0, 0, 0.04);
  --shadow-card-hover: 0 4px 12px rgba(0, 0, 0, 0.08);
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  background: var(--content-bg);
  color: var(--color-text-primary);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
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
  background: var(--sidebar-bg);
  border-right: 1px solid var(--sidebar-border);
  box-shadow: 2px 0 8px rgba(0, 0, 0, 0.15);
  display: flex;
  flex-direction: column;
  z-index: 100;
  overflow: hidden;
}

.sidebar-brand {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 20px 16px 16px;
  border-bottom: 1px solid var(--sidebar-border);
  flex-shrink: 0;
}

.brand-icon {
  width: 34px;
  height: 34px;
  background: var(--color-primary);
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  flex-shrink: 0;
}

.brand-text {
  display: flex;
  flex-direction: column;
  min-width: 0;
}

.brand-name {
  font-size: 0.875rem;
  font-weight: 700;
  color: #f1f5f9;
  letter-spacing: -0.01em;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.brand-subtitle {
  font-size: 0.625rem;
  color: var(--sidebar-section-label);
  text-transform: uppercase;
  letter-spacing: 0.07em;
  margin-top: 1px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.sidebar-nav {
  display: flex;
  flex-direction: column;
  gap: 2px;
  padding: 12px 8px;
  flex-shrink: 0;
}

.nav-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px 10px;
  border-radius: 8px;
  color: var(--sidebar-text);
  text-decoration: none;
  font-size: 0.875rem;
  font-weight: 500;
  transition: all 0.15s ease;
  position: relative;
  white-space: nowrap;
}

.nav-item:hover {
  background: rgba(255, 255, 255, 0.06);
  color: var(--sidebar-text-hover);
}

.nav-item.active {
  background: var(--sidebar-active-bg);
  color: var(--sidebar-active-text);
}

/* Left accent bar for active nav item */
.nav-item.active::before {
  content: '';
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  width: 3px;
  height: 60%;
  background: var(--sidebar-active-indicator);
  border-radius: 0 2px 2px 0;
}

.nav-icon {
  flex-shrink: 0;
  opacity: 0.8;
}

.nav-item.active .nav-icon {
  opacity: 1;
}

.sidebar-filters {
  flex: 1;
  overflow-y: auto;
  /* min-height: 0 is required — without it a flex child cannot shrink below its content height, so overflow-y never triggers */
  min-height: 0;
  padding: 0 8px 8px;
  border-top: 1px solid var(--sidebar-border);
}

.sidebar-section-label {
  font-size: 0.625rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--sidebar-section-label);
  padding: 12px 8px 6px;
}

.sidebar-filters::-webkit-scrollbar {
  width: 4px;
}
.sidebar-filters::-webkit-scrollbar-track {
  background: transparent;
}
.sidebar-filters::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 4px;
}

.sidebar-footer {
  padding: 12px 8px;
  border-top: 1px solid var(--sidebar-border);
  display: flex;
  flex-direction: column;
  gap: 6px;
  flex-shrink: 0;
}

/* ── Main content ─────────────────────────────────────────────────────────── */
.main-content {
  flex: 1;
  margin-left: var(--sidebar-width);
  min-height: 100vh;
  padding: 1.5rem 2rem;
  background: var(--content-bg);
}

/* ── Global page structure ────────────────────────────────────────────────── */
.page-header {
  margin-bottom: 1.5rem;
}

.page-header h2 {
  font-size: 1.875rem;
  font-weight: 700;
  color: var(--color-text-primary);
  margin-bottom: 0.375rem;
  letter-spacing: -0.025em;
}

.page-header p {
  color: var(--color-text-secondary);
  font-size: 0.938rem;
}

/* ── Cards ────────────────────────────────────────────────────────────────── */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.25rem;
  margin-bottom: 1.5rem;
}

.stat-card {
  background: var(--color-surface);
  padding: 1.25rem;
  border-radius: 10px;
  border: 1px solid var(--color-border);
  box-shadow: var(--shadow-card);
  transition: all 0.2s ease;
}

.stat-card:hover {
  border-color: #cbd5e1;
  box-shadow: var(--shadow-card-hover);
}

.stat-label {
  color: var(--color-text-secondary);
  font-size: 0.875rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 0.625rem;
}

.stat-value {
  font-size: 2.25rem;
  font-weight: 700;
  color: var(--color-text-primary);
  letter-spacing: -0.025em;
}

.stat-card.warning .stat-value { color: #ea580c; }
.stat-card.success .stat-value { color: #059669; }
.stat-card.danger .stat-value  { color: #dc2626; }
.stat-card.info .stat-value    { color: #2563eb; }

.card {
  background: var(--color-surface);
  border-radius: 10px;
  padding: 1.25rem;
  border: 1px solid var(--color-border);
  box-shadow: var(--shadow-card);
  margin-bottom: 1.25rem;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  padding-bottom: 0.875rem;
  border-bottom: 1px solid var(--color-border);
}

.card-title {
  font-size: 1.125rem;
  font-weight: 700;
  color: var(--color-text-primary);
  letter-spacing: -0.025em;
}

/* ── Tables ───────────────────────────────────────────────────────────────── */
.table-container {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
}

thead {
  background: #f8fafc;
  border-top: 1px solid var(--color-border);
  border-bottom: 1px solid var(--color-border);
}

th {
  text-align: left;
  padding: 0.5rem 0.75rem;
  font-weight: 600;
  color: #475569;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

td {
  padding: 0.5rem 0.75rem;
  border-top: 1px solid #f1f5f9;
  color: #334155;
  font-size: 0.875rem;
}

tbody tr {
  transition: background-color 0.15s ease;
}

tbody tr:hover {
  background: #f8fafc;
}

/* ── Badges ───────────────────────────────────────────────────────────────── */
.badge {
  display: inline-block;
  padding: 0.313rem 0.75rem;
  border-radius: 6px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.025em;
}

.badge.success    { background: #d1fae5; color: #065f46; }
.badge.warning    { background: #fed7aa; color: #92400e; }
.badge.danger     { background: #fecaca; color: #991b1b; }
.badge.info       { background: #dbeafe; color: #1e40af; }
.badge.increasing { background: #d1fae5; color: #065f46; }
.badge.decreasing { background: #fecaca; color: #991b1b; }
.badge.stable     { background: #e0e7ff; color: #3730a3; }
.badge.high       { background: #fecaca; color: #991b1b; }
.badge.medium     { background: #fed7aa; color: #92400e; }
.badge.low        { background: #dbeafe; color: #1e40af; }

/* ── State utilities ──────────────────────────────────────────────────────── */
.loading {
  text-align: center;
  padding: 3rem;
  color: var(--color-text-secondary);
  font-size: 0.938rem;
}

.error {
  background: #fef2f2;
  border: 1px solid #fecaca;
  color: #991b1b;
  padding: 1rem;
  border-radius: 8px;
  margin: 1rem 0;
  font-size: 0.938rem;
}
```

---

## File 2: client/src/components/FilterBar.vue

### Template block — DO NOT CHANGE
### Script block — DO NOT CHANGE

### Style scoped block — replace entirely

Replace the entire `<style scoped>` block with:

```css
/* FilterBar is rendered inside .sidebar-filters — no sticky positioning or background needed */
.filters-bar {
  /* inherits sidebar context */
}

.filters-container {
  display: flex;
  flex-direction: column;
  gap: 6px;
  padding: 0 2px;
}

.filters-grid {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.filter-group {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.filter-group label {
  font-size: 0.625rem;
  font-weight: 700;
  color: #64748b;
  text-transform: uppercase;
  letter-spacing: 0.06em;
  padding-left: 2px;
}

.filter-select {
  width: 100%;
  padding: 6px 28px 6px 8px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 6px;
  font-size: 0.75rem;
  color: #e2e8f0;
  background-color: rgba(255, 255, 255, 0.06);
  /* Custom chevron for dark background */
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' viewBox='0 0 12 12'%3E%3Cpath d='M3 5l3 3 3-3' stroke='%2394a3b8' stroke-width='1.5' fill='none' stroke-linecap='round'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: right 8px center;
  cursor: pointer;
  transition: all 0.15s ease;
  font-weight: 500;
  font-family: inherit;
  appearance: none;
  -webkit-appearance: none;
}

.filter-select:hover {
  border-color: rgba(255, 255, 255, 0.2);
  background-color: rgba(255, 255, 255, 0.1);
}

.filter-select:focus {
  outline: none;
  border-color: #3b82f6;
  box-shadow: 0 0 0 2px rgba(59, 130, 246, 0.2);
}

.filter-select option {
  background: #1e293b;
  color: #e2e8f0;
}

.reset-filters-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  width: 100%;
  padding: 6px 8px;
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 6px;
  color: #64748b;
  cursor: pointer;
  transition: all 0.15s ease;
  font-size: 0.688rem;
  font-weight: 600;
  font-family: inherit;
  margin-top: 4px;
}

.reset-filters-btn:hover:not(:disabled) {
  background: rgba(255, 255, 255, 0.08);
  border-color: rgba(255, 255, 255, 0.15);
  color: #94a3b8;
}

.reset-filters-btn:disabled {
  opacity: 0.25;
  cursor: not-allowed;
}

.reset-filters-btn svg {
  width: 14px;
  height: 14px;
}
```

---

## File 3: client/src/components/ProfileMenu.vue

### Template block — DO NOT CHANGE
### Script block — DO NOT CHANGE

### Style scoped block — append the following to the END of the existing `<style scoped>` block (do not replace existing rules)

```css
/* Sidebar footer overrides: full-width dark-themed button */
.profile-button {
  width: 100%;
  background: rgba(255, 255, 255, 0.06);
  border: 1px solid rgba(255, 255, 255, 0.1);
  color: #e2e8f0;
  justify-content: flex-start;
}

.profile-button:hover {
  background: rgba(255, 255, 255, 0.1);
  border-color: rgba(255, 255, 255, 0.18);
}

.profile-name {
  color: #e2e8f0;
}

/* Dropdown opens upward — button is at the bottom of a fixed 100vh sidebar */
.dropdown-menu {
  bottom: calc(100% + 0.5rem);
  top: auto;
  right: 0;
  left: 0;
  min-width: unset;
  width: 220px;
}
```

---

## File 4: client/src/components/LanguageSwitcher.vue

### Template block — DO NOT CHANGE
### Script block — DO NOT CHANGE

### Style scoped block — append the following to the END of the existing `<style scoped>` block (do not replace existing rules)

```css
/* Sidebar footer overrides: full-width dark-themed button */
.language-button {
  width: 100%;
  background: rgba(255, 255, 255, 0.06);
  border: 1px solid rgba(255, 255, 255, 0.1);
  color: #94a3b8;
  justify-content: flex-start;
}

.language-button:hover {
  background: rgba(255, 255, 255, 0.1);
  border-color: rgba(255, 255, 255, 0.18);
  color: #e2e8f0;
}

.globe-icon {
  color: #64748b;
}

/* Dropdown opens upward — button is at the bottom of a fixed 100vh sidebar */
.dropdown-menu {
  bottom: calc(100% + 0.5rem);
  top: auto;
  right: 0;
  left: 0;
  min-width: unset;
  width: 160px;
}
```

---

## Verification

After making all changes:
1. Run `npm run dev` in `client/` — confirm no build errors
2. Open http://localhost:3000 — confirm sidebar is visible on the left with dark background
3. Confirm main content is offset to the right (not overlapping sidebar)
4. Click all 6 nav links — confirm routing works and active link shows the blue left-bar indicator
5. Use all 4 filters — confirm data changes in views (useFilters singleton is preserved)
6. Click reset — confirm filters clear
7. Open ProfileMenu dropdown — confirm it opens upward
8. Open LanguageSwitcher — confirm it opens upward and switching EN/JA works
9. Confirm sidebar stays fixed while scrolling long pages
10. Run: grep -r "top: 70px" client/src — should return zero results
