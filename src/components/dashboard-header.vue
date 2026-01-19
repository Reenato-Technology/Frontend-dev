<template>
  <header class="border bg-gray-100 m-3 md:m-5 flex items-center gap-3 rounded-xl border-gray-300 px-4 md:px-5 py-2">
    <button
      @click="$emit('toggleSidebar')"
      class="md:hidden p-2 hover:bg-gray-200 rounded-lg transition-colors"
      aria-label="Toggle menu"
    >
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <line x1="4" x2="20" y1="12" y2="12"/>
        <line x1="4" x2="20" y1="6" y2="6"/>
        <line x1="4" x2="20" y1="18" y2="18"/>
      </svg>
    </button>
    <h1 class="text-lg md:text-xl">Roles</h1>
  </header>
</template>

<script setup lang="ts">
defineEmits(['toggleSidebar'])
</script>