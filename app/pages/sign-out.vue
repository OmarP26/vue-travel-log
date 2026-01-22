<script setup lang="ts">
const authStore = useAuthStore();

onMounted(() => {
  authStore.signOut();
});
</script>

<template>
  <div class="card bg-base-300 container mt-4 min-h-72 text-center mx-auto flex flex-col gap-4 justify-center items-center">
    <span class="loading loading-ring loading-xl" />
  </div>
</template>
