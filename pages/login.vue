<script setup>
definePageMeta({
  layout: "login",
});

import { useForm } from "vee-validate";
import { toTypedSchema } from "@vee-validate/zod";
import * as z from "zod";

import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from "@/components/ui/card";

import { Input } from "@/components/ui/input";

import {
  FormControl,
  FormDescription,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from "@/components/ui/form";

const supabase = useSupabaseClient();
const redirectTo = "http://localhost:3000/confirm";
const emailRedirectTo = "http://localhost:3000/confirm";

if (useSupabaseSession()) {
  navigateTo("/app");
}

const signInWithGoogle = async () => {
  try {
    const { error } = await supabase.auth.signInWithOAuth({
      provider: "google",
      options: {
        redirectTo,
      },
    });
    if (error) throw error;
  } catch (error) {
    console.log(error.message);
  }
};

const signInWithDiscord = async () => {
  try {
    const { error } = await supabase.auth.signInWithOAuth({
      provider: "discord",
      options: {
        redirectTo,
      },
    });
    if (error) throw error;
  } catch (error) {
    console.log(error.message);
  }
};

const userEmail = ref("");

const signInWithEmail = async (email) => {
  try {
    const { error } = await supabase.auth.signInWithOtp({
      email,
      options: {
        emailRedirectTo,
      },
      // Send toast
    });
    if (error) throw error;
  } catch (error) {
    console.log(error.message);
    // Send toast
  }
};

const formSchema = toTypedSchema(
  z.object({
    email: z.string().email(),
  })
);

const form = useForm({
  validationSchema: formSchema,
});

const submitEmail = form.handleSubmit((values) => {
  signInWithEmail(values.email);
});
</script>

<template>
  <div class="w-full h-full flex flex-col items-center">
    <NuxtLink to="/" class="flex items-end space-x-1 h-1/3 pb-8">
      <!-- <Icon name="solar:bolt-bold" class="text-4xl" /> -->
      <h1 class="font-bold text-3xl">💻 website</h1>
    </NuxtLink>

    <div class="w-full max-w-sm">
      <Card class="h-full w-full flex flex-col items-center">
        <CardHeader>
          <CardTitle>Log In or Sign Up</CardTitle>
        </CardHeader>
        <CardContent class="w-full flex flex-col items-center space-y-2">
          <Button
            @click="signInWithGoogle"
            variant="secondary"
            class="w-full space-x-2 flex items-center"
          >
            <Icon name="flowbite:google-solid" class="w-6 h-6" />
            <span>Continue with Google</span>
          </Button>
          <Button
            @click="signInWithDiscord"
            variant="secondary"
            class="w-full space-x-2 flex items-center"
          >
            <Icon name="ic:baseline-discord" class="w-6 h-6" />
            <span>Continue with Discord</span>
          </Button>
          <span>or</span>

          <form @submit.prevent="submitEmail" class="w-full space-y-2">
            <FormField v-slot="{ componentField }" name="email">
              <FormItem>
                <FormControl>
                  <Input
                    type="email"
                    placeholder="Continue with email"
                    v-bind="componentField"
                    class="w-full"
                  />
                </FormControl>
                <FormMessage />
              </FormItem>
            </FormField>
            <Button type="submit" class="w-full"> Send Magic Link </Button>
          </form>
        </CardContent>
      </Card>
    </div>
  </div>
</template>
