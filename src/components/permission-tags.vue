<template>
  <button
    @click="handleClick"
    class="inline-flex items-center gap-2 px-3 py-2 border rounded-lg font-medium text-sm transition-all duration-200"
    :class="
      removable
        ? 'border-gray-200 text-gray-900 hover:bg-gray-200'
        : 'border-gray-200 text-gray-900 hover:bg-gray-200'
    "
  >
    <span>{{ permission.name }}</span>
    <span v-if="removable" class="text-red-500 font-bold">×</span>
    <span v-else class="text-green-500 font-bold">+</span>
  </button>
</template>

<script setup lang="ts" name="PermissionTag">
import type { Permission } from '../types'

const props = defineProps<{
  permission: Permission
  removable: boolean
}>()

const emit = defineEmits<{
  remove: [permissionId: string]
  add: [permission: Permission]
}>()

const handleClick = (evt: Event) => {
  evt.preventDefault()
  if (props.removable) {
    emit('remove', props.permission.id)
  } else {
    emit('add', props.permission)
  }
}
</script>
