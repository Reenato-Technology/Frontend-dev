<template>
  <div class="flex h-screen bg-gray-900">
    <Sidebar :isOpen="isSidebarOpen" @close="isSidebarOpen = false" />
    <main class="flex-1 flex flex-col bg-white rounded-none md:rounded-3xl md:m-3 overflow-hidden">
      <Header @toggleSidebar="isSidebarOpen = !isSidebarOpen" />
      <div class="flex-1 overflow-y-auto">
        <RolesPage />
      </div>
    </main>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import Sidebar from './components/sidebar-items.vue'
import Header from './components/dashboard-header.vue'
import RolesPage from './pages/roles.vue'

const isSidebarOpen = ref(false)
</script>