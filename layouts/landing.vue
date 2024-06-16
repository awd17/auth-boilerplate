<script setup>
import { Button } from "@/components/ui/button";
import {
  Sheet,
  SheetContent,
  SheetDescription,
  SheetHeader,
  SheetTitle,
  SheetTrigger,
} from "@/components/ui/sheet";

const loggedIn = ref(useSupabaseSession());
</script>

<template>
  <header>
    <nav class="max-w-scren-xl flex items-center justify-between mx-auto p-4">
      <NuxtLink to="/" class="flex items-center space-x-1 md:w-1/4">
        <!-- <Icon name="solar:bolt-bold" class="text-3xl" /> -->
        <h1 class="font-bold text-2xl">💻 website</h1>
      </NuxtLink>

      <div class="items-center justify-between hidden md:flex w-full md:w-auto">
        <ul class="flex space-x-8">
          <li>
            <NuxtLink to="#">Features</NuxtLink>
          </li>
          <li>
            <NuxtLink to="#">Pricing</NuxtLink>
          </li>
          <li>
            <NuxtLink to="#">Resources</NuxtLink>
          </li>
        </ul>
      </div>

      <div class="flex space-x-2 md:w-1/4 justify-end">
        <NuxtLink to="/login" v-if="!loggedIn">
          <Button variant="secondary">Log In</Button>
        </NuxtLink>
        <NuxtLink to="/login" v-if="!loggedIn">
          <Button>Get Started</Button>
        </NuxtLink>
        <NuxtLink to="/app" v-if="loggedIn">
          <Button>Go to app</Button>
        </NuxtLink>

        <Sheet>
          <SheetTrigger>
            <Button variant="ghost" size="icon" class="md:hidden">
              <Icon name="solar:hamburger-menu-linear" class="text-3xl" />
            </Button>
          </SheetTrigger>
          <SheetContent>
            <nav class="grid gap-6 text-lg font-medium">
              <NuxtLink to="/" class="hover:text-foreground"> Home </NuxtLink>
              <NuxtLink
                to="/"
                class="text-muted-foreground hover:text-foreground"
              >
                Features
              </NuxtLink>
              <NuxtLink
                to="/"
                class="text-muted-foreground hover:text-foreground"
              >
                Pricing
              </NuxtLink>
              <NuxtLink
                to="/"
                class="text-muted-foreground hover:text-foreground"
              >
                Resources
              </NuxtLink>
            </nav>
          </SheetContent>
        </Sheet>
      </div>
    </nav>
  </header>
  <slot />
</template>
