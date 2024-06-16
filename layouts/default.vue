<script setup>
import { Button } from "@/components/ui/button";

const user = useSupabaseUser();
const client = useSupabaseClient();

async function logout() {
  const { error } = await client.auth.signOut();
  if (error) console.log(error);
  return navigateTo("/");
}
</script>

<template>
  <header>
    <nav class="max-w-scren-xl flex items-center justify-between mx-auto p-4">
      <NuxtLink to="/" class="flex items-center space-x-1 md:w-1/4">
        <!-- <Icon name="solar:bolt-bold" class="text-3xl" /> -->
        <h1 class="font-bold text-2xl">💻 website</h1>
      </NuxtLink>

      <div class="flex space-x-2 md:w-1/4 justify-end">
        <Button variant="secondary" @click="logout">Log out</Button>
      </div>
    </nav>
  </header>
  <slot />
</template>
