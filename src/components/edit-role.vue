<template>
  <div class="fixed inset-0 bg-black/25 bg-opacity-50 flex items-center justify-center z-50">
    <div class="bg-white rounded-lg shadow-lg max-w-3xl w-full mx-4 max-h-[90vh] overflow-y-auto">
      <div class="sticky top-0 bg-white border-b border-gray-200 px-8 py-6 flex items-center justify-between">
        <div>
          <h2 class="text-2xl font-bold text-gray-900">Edit Role</h2>
          <p class="text-gray-600 text-sm mt-1">Edit the name of the role and set permissions.</p>
        </div>
        <button
          @click="emit('close')"
          class="text-gray-400 hover:text-gray-600 text-2xl font-light"
        >
          ×
        </button>
      </div>

      <div class="px-8 py-6 space-y-5">
         <div class="bg-[#C382011A] text-[#854D0F] border border-gray-50 rounded-xl p-3 flex items-center gap-3">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-circle-alert-icon lucide-circle-alert"><circle cx="12" cy="12" r="10"/><line x1="12" x2="12" y1="8" y2="12"/><line x1="12" x2="12.01" y1="16" y2="16"/></svg>
          <p class="text-sm font-semibold">The permission list will be updated when roles are modified.</p>
        </div>

        <div>
          <label class="block text-sm text-gray-600 mb-2">Role name</label>
          <input
            v-model="form.name"
            type="text"
            placeholder="Role name"
            class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-900 focus:border-transparent"
          />
        </div>

        <div>
          <h3 class="font-bold text-gray-900 mb-4 uppercase text-sm tracking-wider">Permissions</h3>

          <div class="mb-6">
            <h4 class="text-sm font-semibold text-gray-700 mb-3">Selected Permissions</h4>
            <div v-if="form.permissions.length > 0" class="flex flex-wrap gap-2">
              <PermissionTag
                v-for="permission in form.permissions"
                :key="permission.id"
                :permission="permission"
                :removable="true"
                @remove="removePermission"
              />
            </div>
            <p v-else class="text-gray-400 text-sm">No permissions selected</p>
          </div>

          <div class="space-y-4">
            <div>
              <h4 class="text-sm font-semibold text-primary mb-3">Available Permissions</h4>
              <div class="flex flex-wrap gap-2">
                <PermissionTag
                  v-for="permission in availablePermissions"
                  :key="permission.id"
                  :permission="permission"
                  :removable="false"
                  @add="addPermission"
                />
              </div>
            </div>
          </div>
        </div>

       
      </div>

      <div class="sticky bottom-0 bg-gray-50 border-t border-gray-200 px-8 py-4 flex items-center justify-between gap-3">
        <button
          @click="deleteRole"
          class="px-6 py-2 text-red-600 hover:text-red-700 font-semibold"
        >
          Delete Role
        </button>
        <div class="flex gap-3">
          <button
            @click="emit('close')"
            class="px-6 py-2 bg-white text-gray-900 border border-gray-300 rounded-lg font-semibold hover:bg-gray-50 transition-colors"
          >
            Cancel
          </button>
          <button
            @click="save"
            class="px-6 py-2 main-btn text-white rounded-lg font-semibold hover:opacity-90 transition-opacity"
          >
            Save and Update Changes
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts" name="EditRoleModal">
import { ref, computed } from 'vue'
import PermissionTag from '../components/permission-tags.vue'
import type { Role, Permission } from '../types'
import { allPermissions } from '../constants/permissions';

const props = defineProps<{
  role: Role
}>()

const emit = defineEmits<{
  close: []
  save: [role: Role]
  delete: [role: Role]
}>()



const form = ref<Role>({
  ...props.role
})

const availablePermissions = computed(() => {
  const selectedIds = new Set(form.value.permissions.map((p) => p.id))
  return allPermissions.filter((p) => !selectedIds.has(p.id))
})

const addPermission = (permission: Permission) => {
  if (!form.value.permissions.find((p) => p.id === permission.id)) {
    form.value.permissions.push(permission)
  }
}

const removePermission = (permissionId: string) => {
  form.value.permissions = form.value.permissions.filter((p) => p.id !== permissionId)
}

const deleteRole = () => {
  emit('delete', props.role)
}

const save = () => {
  if (form.value.name.trim()) {
    emit('save', form.value)
  }
}
</script>
