<template>
  <div class="fixed inset-0 bg-black/25 bg-opacity-50 flex items-center justify-center z-50">
    <div class="bg-white rounded-lg shadow-lg max-w-sm w-full mx-4">
      <div class="p-8 text-center">
        <div class="w-16 h-16 bg-red-100 rounded-full flex items-center justify-center mx-auto mb-4">
          <svg class="w-8 h-8 text-red-600" fill="currentColor" viewBox="0 0 20 20">
            <path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd" />
          </svg>
        </div>
        <h3 class="text-xl font-bold text-gray-900 mb-2">Delete '{{ role.name }}' Role?</h3>
        <p class="text-gray-600 text-sm mb-6">Deleting this role will remove all associated permissions and access rights. This action cannot be undone.</p>
        <div class="flex gap-3">
          <button
            @click="emit('close')"
            class="flex-1 px-4 py-2 bg-white text-gray-900 border border-gray-300 rounded-lg font-semibold hover:bg-gray-50 transition-colors"
          >
            Cancel
          </button>
          <button
            @click="confirm"
            class="flex-1 px-4 py-2 bg-red-600 text-white rounded-lg font-semibold hover:bg-red-700 transition-colors"
          >
            Yes
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts" name="DeleteConfirmModal">
import type { Role } from '../types'

defineProps<{
  role: Role
}>()

const emit = defineEmits<{
  close: []
  confirm: []
}>()

const confirm = () => {
  emit('confirm')
}
</script>
