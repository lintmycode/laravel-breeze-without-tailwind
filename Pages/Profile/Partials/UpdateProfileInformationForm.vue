<script setup>
import InputError from '@/Components/InputError.vue'
import InputLabel from '@/Components/InputLabel.vue'
import PrimaryButton from '@/Components/PrimaryButton.vue'
import TextInput from '@/Components/TextInput.vue'
import { Link, useForm, usePage } from '@inertiajs/vue3'

const props = defineProps({
  mustVerifyEmail: Boolean,
  status: String
})

const user = usePage().props.auth.user

const form = useForm({
  name: user.name,
  email: user.email
})
</script>

<template>
  <section>
    <header>
      <h2>Profile Information</h2>

      <p>Update your account's profile information and email address.</p>
    </header>

    <form @submit.prevent="form.patch(route('profile.update'))">
      <div>
        <InputLabel for="name" value="Name" />

        <TextInput
          id="name"
          type="text"
          v-model="form.name"
          required
          autofocus
          autocomplete="name"
        />

        <InputError :message="form.errors.name" />
      </div>

      <div>
        <InputLabel for="email" value="Email" />

        <TextInput id="email" type="email" v-model="form.email" required autocomplete="username" />

        <InputError :message="form.errors.email" />
      </div>

      <div v-if="props.mustVerifyEmail && user.email_verified_at === null">
        <p>
          Your email address is unverified.
          <Link :href="route('verification.send')" method="post" as="button">
            Click here to re-send the verification email.
          </Link>
        </p>

        <div v-show="props.status === 'verification-link-sent'">
          A new verification link has been sent to your email address.
        </div>
      </div>

      <div>
        <PrimaryButton :disabled="form.processing">Save</PrimaryButton>

        <Transition>
          <p v-if="form.recentlySuccessful">Saved.</p>
        </Transition>
      </div>
    </form>
  </section>
</template>
