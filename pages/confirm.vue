<script setup>
const user = useSupabaseUser();

watch(
  user,
  () => {
    if (user.value) {
      // Redirect to protected page
      return navigateTo("/app");
    }
  },
  { immediate: true }
);
</script>

<template>
  <div>Waiting for login...</div>
</template>
