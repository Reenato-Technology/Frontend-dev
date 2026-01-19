<template>
  <!-- This is the mobile overlay -->
  <Transition name="overlay">
    <div
      v-if="isOpen"
      @click="$emit('close')"
      class="fixed inset-0 bg-black/25 bg-opacity-50 z-40 md:hidden"
    ></div>
  </Transition>

 <aside
    :class="[
      'bg-sidebar text-gray-100 flex flex-col transition-transform duration-300 ease-in-out z-50',
      'md:relative md:translate-x-0 md:w-56 lg:w-64',
      'fixed inset-y-0 left-0 w-75',
      isOpen ? 'translate-x-0' : '-translate-x-full'
    ]"
  >
    <!-- Mobile close button for mobile menu -->
    <div class="md:hidden absolute top-4 right-4">
      <button
        @click="$emit('close')"
        class="p-2 hover:bg-gray-800 rounded-lg transition-colors"
        aria-label="Close menu"
      >
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <path d="M18 6 6 18"/>
          <path d="m6 6 12 12"/>
        </svg>
      </button>
    </div>

    <div class="p-6 border-b border-gray-800">
      <div class="text-2xl font-bold flex items-center gap-3">
       <img src="../assets/logo.png" alt="Close" class="size-8" />
      <h1 class="text-2xl!">IOAnts</h1>
      </div>
    </div>

    <nav class="flex-1 py-6 px-3 space-y-1 overflow-y-auto">
      <NavItem label="Overview" icon="overview" />
      <NavItem label="Service Requests" icon="service" />
      <NavItem label="Tenancy Applications" icon="tenancy" />

      <NavItemCollapsible label="My Properties" icon="properties">
        <NavSubItem label="Property 1" />
      </NavItemCollapsible>

      <NavItemCollapsible label="My Tenants" icon="tenants">
        <NavSubItem label="Tenant 1" />
      </NavItemCollapsible>

      <NavItemCollapsible label="Payments" icon="payments">
        <NavSubItem label="Payment 1" />
      </NavItemCollapsible>

      <NavItemCollapsible label="Users" icon="users">
        <NavSubItem label="User 1" />
      </NavItemCollapsible>

      <NavItemCollapsible label="Roles" icon="roles" :expanded="true">
        <NavSubItem label="View Roles" :active="true" />
        <NavSubItem label="Members" />
      </NavItemCollapsible>

      <NavItem label="Notification" icon="notification" />
      <NavItem label="Audit Logs" icon="audit" />
    </nav>

    <div class="p-4 border-t  border-gray-600">
      <div class="flex items-center justify-between cursor-pointer bg-white gap-3 p-3 rounded-lg">
        <div class="flex items-center gap-3">
          <img src="https://images.unsplash.com/photo-1500648767791-00dcc994a43e?w=1600&auto=format&fit=crop&q=60&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MTR8fGF2YXRhcnxlbnwwfHwwfHx8MA%3D%3D" alt="User" class="size-10 rounded-lg object-cover object-top" />
        <div class="flex-1 min-w-0">
          <div class="font-medium text-primary truncate text-sm">Paterson Paul</div>
          <div class="text-gray-800 truncate text-xs">Landlord</div>
        </div>
       
        </div>
        <div class=" bg-gray-100  rounded-md p-0.5 text-primary border border-gray-300">
          <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-ellipsis-icon lucide-ellipsis"><circle cx="12" cy="12" r="1"/><circle cx="19" cy="12" r="1"/><circle cx="5" cy="12" r="1"/></svg>
        </div>
      </div>
    </div>
  </aside>
</template>

<script setup lang="ts" name="SideBar">
import NavItem from '../components/nav/items.vue'
import NavItemCollapsible from '../components/nav/items-collapsible.vue'
import NavSubItem from '../components/nav/sub-items.vue'

defineProps<{
  isOpen: boolean
}>()

defineEmits(['close'])
</script>

<style scoped>
.overlay-enter-active,
.overlay-leave-active {
  transition: opacity 0.3s ease;
}

.overlay-enter-from,
.overlay-leave-to {
  opacity: 0;
}
</style>