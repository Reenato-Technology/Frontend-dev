<template>
  <button class="w-full px-4 py-3 text-left text-sm text-white/55 hover:bg-[#70757A] hover:text-gray-200 rounded-lg transition flex items-center gap-3">
    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" v-html="svgPath"></svg>
    <span>{{ label }}</span>
  </button>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { icons } from '../../constants/icons';

const props = defineProps<{
  icon: string
  label: string
}>()



const svgPath = computed(() => icons[props.icon] || icons['overview'])
</script>