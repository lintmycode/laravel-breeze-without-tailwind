<script setup>
import Checkbox from '@/Components/Checkbox.vue'
import GuestLayout from '@/Layouts/GuestLayout.vue'
import InputError from '@/Components/InputError.vue'
import InputLabel from '@/Components/InputLabel.vue'
import PrimaryButton from '@/Components/PrimaryButton.vue'
import TextInput from '@/Components/TextInput.vue'
import { Head, Link, useForm } from '@inertiajs/vue3'

defineProps({
  canResetPassword: Boolean,
  status: String
})

const form = useForm({
  email: '',
  password: '',
  remember: false
})

const submit = () => {
  form.post(route('login'), {
    onFinish: () => form.reset('password')
  })
}
</script>

<template>
  <GuestLayout>
    <Head title="Log in" />

    <div v-if="status">
      {{ status }}
    </div>

    <form @submit.prevent="submit">
      <div>
        <InputLabel for="email" value="Email" />

        <TextInput
          id="email"
          type="email"
          v-model="form.email"
          required
          autofocus
          autocomplete="username"
        />

        <InputError :message="form.errors.email" />
      </div>

      <div>
        <InputLabel for="password" value="Password" />

        <TextInput
          id="password"
          type="password"
          v-model="form.password"
          required
          autocomplete="current-password"
        />

        <InputError :message="form.errors.password" />
      </div>

      <div>
        <label>
          <Checkbox name="remember" v-model:checked="form.remember" />
          <span>Remember me</span>
        </label>
      </div>

      <div>
        <Link v-if="canResetPassword" :href="route('password.request')">
          Forgot your password?
        </Link>

        <PrimaryButton :class="{ processing: form.processing }" :disabled="form.processing">
          Log in
        </PrimaryButton>
      </div>
    </form>
  </GuestLayout>
</template>
