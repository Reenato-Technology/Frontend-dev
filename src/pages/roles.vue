<template>
  <div class="space-y-6 border min-h-[85vh] border-gray-200 mx-3 md:mx-5 rounded-xl">
    <div class="rounded py-4 md:py-6 border-b border-gray-200">
      <div class="flex flex-col lg:flex-row items-start lg:items-center lg:justify-between px-3 gap-4">
        <div class="flex items-center gap-3">
          <div class="text-center border border-gray-200 rounded-lg p-2">
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-users-round-icon lucide-users-round">
              <path d="M18 21a8 8 0 0 0-16 0"/>
              <circle cx="10" cy="8" r="5"/>
              <path d="M22 20c0-3.37-2-6.5-4-8a5 5 0 0 0-.45-8.3"/>
            </svg>
          </div>
          <p class="text-lg font-medium">Add Roles</p>
        </div>
        
        <div class="flex flex-col sm:flex-row items-stretch sm:items-center gap-3 w-full lg:flex-1 lg:max-w-2xl">
          <div class="relative flex-1">
            <svg class="w-5 h-5 text-gray-400 absolute left-3 top-1/2 -translate-y-1/2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
            </svg>
            <input
              type="text"
              v-model="searchQuery"
              placeholder="Search roles..."
              class="w-full pl-10 pr-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-1 focus:ring-primary text-sm"
            />
          </div>
          
          <button
            @click="openAddModal"
            class="px-4 sm:px-6 py-2 main-btn hover:opacity-90 transition-opacity flex items-center justify-center gap-2 text-sm whitespace-nowrap"
          >
            <span>+</span> Add new Role
          </button>
        </div>
      </div>
    </div>

    <!-- No roles at all -->
    <div v-if="roles.length === 0" class="flex flex-col items-center justify-center px-4 sm:px-16 py-20 sm:py-40 text-center">
      <div class="text-center border border-gray-200 rounded-lg p-3 mb-3">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-users-round-icon lucide-users-round">
          <path d="M18 21a8 8 0 0 0-16 0"/>
          <circle cx="10" cy="8" r="5"/>
          <path d="M22 20c0-3.37-2-6.5-4-8a5 5 0 0 0-.45-8.3"/>
        </svg>
      </div>
      <h3 class="text-lg font-semibold text-gray-900 mb-2">No Roles</h3>
      <p class="text-gray-500 text-sm">Create your first role to get started</p>
    </div>

    <!-- Search returned no results -->
    <div v-else-if="filteredRoles.length === 0" class="flex flex-col items-center justify-center px-4 sm:px-16 py-20 sm:py-40 text-center">
      <div class="text-center border border-gray-200 rounded-lg p-3 mb-3">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
          <circle cx="11" cy="11" r="8"/>
          <path d="m21 21-4.3-4.3"/>
        </svg>
      </div>
      <h3 class="text-lg font-semibold text-gray-900 mb-2">No roles found</h3>
      <p class="text-gray-500 text-sm">No roles match "{{ searchQuery }}"</p>
      <button @click="searchQuery = ''" class="mt-4 text-primary hover:text-primary/80 text-sm font-medium">
        Clear search
      </button>
    </div>

    <!-- Table with results -->
    <div v-else class="bg-white rounded-lg border border-gray-200 overflow-hidden mx-3 md:mx-0">
      <div class="overflow-x-auto">
        <table class="w-full">
          <thead class="bg-gray-50 border-b border-gray-200">
            <tr>
              <th class="px-4 md:px-6 py-3 text-left text-xs font-semibold text-gray-700 uppercase tracking-wide">S/N</th>
              <th class="px-4 md:px-6 py-3 text-left text-xs font-semibold text-gray-700 uppercase tracking-wide">Roles</th>
              <th class="px-4 md:px-6 py-3 text-left text-xs font-semibold text-gray-700 uppercase tracking-wide">Permissions</th>
              <th class="px-4 md:px-6 py-3 text-left text-xs font-semibold text-gray-700 uppercase tracking-wide hidden sm:table-cell">Created by</th>
              <th class="px-4 md:px-6 py-3 text-left text-xs font-semibold text-gray-700 uppercase tracking-wide hidden md:table-cell">Date created</th>
              <th class="px-4 md:px-6 py-3 text-left text-xs font-semibold text-gray-700 uppercase tracking-wide">Action</th>
            </tr>
          </thead>
          <tbody class="divide-y divide-gray-200">
            <tr v-for="(role, index) in filteredRoles" :key="role.id" class="hover:bg-gray-50 transition-colors">
              <td class="px-4 md:px-6 py-4 text-sm">{{ index + 1 }}</td>
              <td class="px-4 md:px-6 py-4 text-sm capitalize font-medium">{{ role.name }}</td>
              <td class="px-4 md:px-6 py-4 text-sm">{{ role.permissions.length }}</td>
              <td class="px-4 md:px-6 py-4 text-sm hidden sm:table-cell">{{ role.createdBy || 'Admin' }}</td>
              <td class="px-4 md:px-6 py-4 text-sm hidden md:table-cell">{{ formatDate(role.createdAt) }}</td>
              <td class="px-4 md:px-6 py-4 text-sm">
                <button
                  @click="openEditModal(role)"
                  class="text-primary hover:underline cursor-pointer font-medium"
                >
                  Edit
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <AddRoleModal v-if="isAddModalOpen" @close="closeAddModal" @save="saveRole" />
    <EditRoleModal v-if="isEditModalOpen && editingRole" :role="editingRole" @close="closeEditModal" @save="updateRole" @delete="confirmDelete" />
    <DeleteConfirmModal v-if="isDeleteModalOpen && roleToDelete" :role="roleToDelete" @close="closeDeleteModal" @confirm="executeDelete" />
  </div>
</template>

<script setup lang="ts" name="RolesPage">
import { ref, computed } from 'vue'
import AddRoleModal from '../components/add-role.vue'
import EditRoleModal from '../components/edit-role.vue'
import DeleteConfirmModal from '../components/delete-role.vue'
import type { Role } from '../types'

const roles = ref<Role[]>([])
const searchQuery = ref('')
const isAddModalOpen = ref(false)
const isEditModalOpen = ref(false)
const isDeleteModalOpen = ref(false)
const editingRole = ref<Role | null>(null)
const roleToDelete = ref<Role | null>(null)

const filteredRoles = computed(() => {
  if (!searchQuery.value.trim()) {
    return roles.value
  }
  return roles.value.filter((role) =>
    role.name.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})

const openAddModal = () => {
  isAddModalOpen.value = true
}

const closeAddModal = () => {
  isAddModalOpen.value = false
}

const openEditModal = (role: Role) => {
  editingRole.value = { ...role }
  isEditModalOpen.value = true
}

const closeEditModal = () => {
  isEditModalOpen.value = false
  editingRole.value = null
}

const saveRole = (role: Role) => {
  const newRole: Role = {
    ...role,
    id: Date.now().toString(),
    createdBy: 'Admin',
    createdAt: new Date().toISOString()
  }
  roles.value.push(newRole)
  isAddModalOpen.value = false
}

const updateRole = (updatedRole: Role) => {
  const index = roles.value.findIndex((r) => r.id === updatedRole.id)
  if (index !== -1) {
    roles.value[index] = updatedRole
  }
  closeEditModal()
}

const confirmDelete = (role: Role) => {
  roleToDelete.value = role
  isDeleteModalOpen.value = true
  closeEditModal()
}

const closeDeleteModal = () => {
  isDeleteModalOpen.value = false
  roleToDelete.value = null
}

const executeDelete = () => {
  if (roleToDelete.value) {
    roles.value = roles.value.filter((r) => r.id !== roleToDelete.value!.id)
  }
  closeDeleteModal()
}

const formatDate = (dateString: string | undefined) => {
  if (!dateString) return new Date().toLocaleDateString('en-GB', { day: '2-digit', month: '2-digit', year: 'numeric' })
  const date = new Date(dateString)
  return date.toLocaleDateString('en-GB', { day: '2-digit', month: '2-digit', year: 'numeric' })
}
</script>