<template>
  <div class="fixed inset-0 bg-black/25 bg-opacity-50 flex items-center justify-center z-50">
    <div class="bg-white rounded-lg shadow-lg max-w-2xl w-full mx-4 max-h-[90vh] overflow-y-auto">
      <div class="sticky top-0 bg-white border-b border-gray-200 px-8 py-6 flex items-center justify-between">
        <div>
          <h2 class="text-2xl font-bold text-gray-900">Add New Role</h2>
          <p class="text-gray-600 text-sm mt-1">Enter Role name and set Permissions for the role</p>
        </div>
        <button @click="closeModal" class="text-gray-400 hover:text-gray-600 text-xl font-light">
          ×
        </button>
      </div>

      <div class="px-8 py-6 space-y-8">
        <div>
          <label class="block text-sm font-medium text-gray-600 mb-2">Role name</label>
          <input v-model="form.name" @focus="inputState = 'active'" @blur="handleBlur" @input="inputState = 'typing'"
            type="text" placeholder="Add role name"
            class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-1 focus:ring-primary focus:border-transparent"
            :class="{
              'bg-gray-50': inputState === 'inactive',
              'bg-white': inputState !== 'inactive',
              'border-red-300': showError && !form.name.trim()
            }" />
          <p v-if="showError && !form.name.trim()" class="text-red-500 text-xs mt-1">
            Role name is required
          </p>
        </div>

        <div>
          <h3 class="font-bold mb-4 uppercase text-sm tracking-wider">Permissions</h3>

          <div class="mb-6">
            <h4 class="text-sm font-semibold text-gray-700 mb-3">Selected Permissions</h4>
            <div v-if="form.permissions.length > 0" class="flex bg-gray-100 p-3 rounded-lg flex-wrap gap-2">
              <PermissionTag v-for="permission in form.permissions" :key="permission.id" :permission="permission"
                :removable="true" @remove="removePermission" />
            </div>
            <p v-else class="text-gray-400 text-sm">No permissions selected</p>
          </div>

          <div class="space-y-4">
            <div>
              <h4 class="text-sm font-semibold text-gray-700 mb-3">Available Permissions</h4>
              <div class="flex flex-wrap gap-2">
                <PermissionTag v-for="permission in availablePermissions" :key="permission.id" :permission="permission"
                  :removable="false" @add="addPermission" />
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="sticky bottom-0 bg-gray-50 border-t border-gray-200 px-8 py-4 flex items-center justify-end gap-3">
        <button @click="closeModal"
          class="px-6 py-2 bg-white rounded-xl border border-gray-300 rounded-button font-semibold hover:bg-primary/5 transition-colors">
          Cancel
        </button>
        <button @click="save" :disabled="!canSave"
          class="px-6 py-2 main-btn hover:opacity-90 transition-opacity disabled:opacity-50 disabled:cursor-not-allowed">
          Add new role
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts" name="AddRoleModal">
import { ref, computed } from 'vue'
import PermissionTag from '../components/permission-tags.vue'
import type { Role, Permission, InputState } from '../types'
import { allPermissions } from '../constants/permissions'

const emit = defineEmits<{
  close: []
  save: [role: Role]
}>()

const inputState = ref<InputState>('inactive')
const showError = ref(false)

const form = ref<Role>({
  id: '',
  name: '',
  permissions: []
})

const handleBlur = () => {
  inputState.value = form.value.name ? 'active' : 'inactive'
  showError.value = true
}

const availablePermissions = ref<Permission[]>([...allPermissions])

const canSave = computed(() => {
  return form.value.name.trim().length > 0
})

const addPermission = (permission: Permission) => {
  if (!form.value.permissions.find((p) => p.id === permission.id)) {
    form.value.permissions.push(permission)
    availablePermissions.value = availablePermissions.value.filter(
      (p) => p.id !== permission.id
    )
  }
}

const removePermission = (permissionId: string) => {
  const permission = form.value.permissions.find((p) => p.id === permissionId)
  if (permission) {
    form.value.permissions = form.value.permissions.filter(
      (p) => p.id !== permissionId
    )
    availablePermissions.value.push(permission)
    availablePermissions.value.sort((a, b) => {
      const indexA = allPermissions.findIndex((p) => p.id === a.id)
      const indexB = allPermissions.findIndex((p) => p.id === b.id)
      return indexA - indexB
    })
  }
}

const closeModal = () => {
  emit('close')
}

const save = () => {
  if (canSave.value) {

    const roleToSave: Role = {
      id: '',
      name: form.value.name.trim(),
      permissions: [...form.value.permissions],
      createdBy: undefined,
      createdAt: undefined
    }

    emit('save', roleToSave)

    form.value = { id: '', name: '', permissions: [] }
    availablePermissions.value = [...allPermissions]
  }
}
</script>