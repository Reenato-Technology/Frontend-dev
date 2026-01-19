<template>
  <button 
    :class="[
      'w-full px-4 py-2 text-left text-sm rounded-lg transition flex items-center gap-3',
      active 
        ? 'text-white bg-[#70757A]' 
        : 'text-white/55 hover:text-gray-300 hover:bg-[#70757A]'
    ]"
  >
    <span><svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-corner-down-right-icon lucide-corner-down-right"><path d="m15 10 5 5-5 5"/><path d="M4 4v7a4 4 0 0 0 4 4h12"/></svg></span>
    <span>{{ label }}</span>
  </button>
</template>

<script setup lang="ts">
defineProps<{
  label: string
  active?: boolean
}>()
</script>