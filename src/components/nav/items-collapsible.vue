<template>
  <div>
    <button
      @click="isOpen = !isOpen"
      class="w-full px-4 py-3 text-left text-sm text-white/55 hover:bg-[#70757A] hover:text-white rounded-lg transition flex items-center justify-between"
    >
      <div class="flex items-center gap-3">
        <component :is="iconComponent" :size="20" />
        <span>{{ label }}</span>
      </div>
      <ChevronDown
        :size="16"
        :class="['transition-transform duration-200', isOpen ? 'rotate-180' : '']"
      />
    </button>
    
    <div v-if="isOpen" class="pl-4 space-y-1 mt-2">
      <slot />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import { 
  ChevronDown,
  Home,
  Users,
  CreditCard,
  User,
  Shield
} from 'lucide-vue-next'

const props = withDefaults(
  defineProps<{
    icon: string
    label: string
    expanded?: boolean
  }>(),
  {
    expanded: false,
  }
)

const isOpen = ref(props.expanded)

const iconMap: Record<string, any> = {
  'properties': Home,
  'tenants': Users,
  'payments': CreditCard,
  'users': User,
  'roles': Shield
}

const iconComponent = computed(() => iconMap[props.icon] || Home)
</script>