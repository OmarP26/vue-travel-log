<script setup lang="ts">
const route = useRoute();
const error = route.query.error || "Sowy unkown error occured";
</script>

<template>
  <div class="card bg-base-300 container mt-4 min-h-72 text-center mx-auto flex flex-col gap-4 justify-center items-center">
    <div role="alert" class="alert alert-error min-w-1/2 text-center">
      <span>Oh no {{ error }}</span>
    </div>
    <NuxtLink to="/" class="btn btn-primary">
      Back to home
      <Icon name="tabler:arrow-left" size="32" />
    </NuxtLink>
  </div>
</template>
