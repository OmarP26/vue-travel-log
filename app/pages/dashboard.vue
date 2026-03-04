<script setup lang="ts">
const isSidebarOpen = ref(true);
const locationsStore = useLocationsStore();
const sidebarStore = useSidebarStore();
const route = useRoute();

onMounted(() => {
  isSidebarOpen.value = localStorage.getItem("isSidebarOpen") === "true";
  if (route.path !== "/dashboard") {
    locationsStore.refresh();
  }
});

function toggleSidebar() {
  isSidebarOpen.value = !isSidebarOpen.value;
  localStorage.setItem("isSidebarOpen", isSidebarOpen.value.toString());
}
</script>

<template>
  <div class="flex-1 flex">
    <div class="bg-base-100 border-2 border-primary transition-all duration-300 shrink-0" :class="{ 'w-64': isSidebarOpen, 'w-18': !isSidebarOpen }">
      <div
        class="flex cursor-pointer hover:bg-base-200"
        :class="{ 'justify-center': !isSidebarOpen, 'justify-end': isSidebarOpen }"
        @click="toggleSidebar"
      >
        <Icon
          v-if="isSidebarOpen"
          name="tabler:chevron-left"
          size="32"
        />
        <Icon
          v-else
          name="tabler:chevron-right"
          size="32"
        />
      </div>
      <div class="flex flex-col">
        <SidebarButton
          label="Locations"
          icon="tabler:map"
          href="/dashboard"
          :show-label="isSidebarOpen"
        />
        <SidebarButton
          label="Add Location"
          icon="tabler:circle-plus-filled"
          href="/dashboard/add"
          :show-label="isSidebarOpen"
        />
        <div v-if="sidebarStore.sidebarItems.length || sidebarStore.loading" class="divider" />
        <div v-if="sidebarStore.loading" class="px-4">
          <div class="skeleton h-4 w-full" />
        </div>
        <div v-if="!sidebarStore.loading && sidebarStore.sidebarItems.length" class="flex flex-col">
          <SidebarButton
            v-for="item in sidebarStore.sidebarItems"
            :key="item.id"
            :label="item.label"
            :icon="item.icon"
            :href="item.href"
            :show-label="isSidebarOpen"
          />
        </div>
        <div class="divider" />
        <SidebarButton
          label="Sign Out"
          icon="tabler:logout-2"
          href="/sign-out"
          :show-label="isSidebarOpen"
        />
      </div>
    </div>
    <div class="flex-1 overflow-auto">
      <div class="flex flex-col size-full">
        <NuxtPage />
        <Map
          class="flex-1"
        />
      </div>
    </div>
  </div>
</template>
