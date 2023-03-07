<script setup>
import { Head, Link } from '@inertiajs/vue3'

defineProps({
  canLogin: Boolean,
  canRegister: Boolean,
  laravelVersion: String,
  phpVersion: String
})
</script>

<template>
  <Head title="Welcome" />

  <Link v-if="$page.props.auth.user" :href="route('dashboard')">Dashboard</Link>

  <Link :href="route('login')">Log in</Link>

  <Link v-if="canRegister" :href="route('register')">Register</Link>
</template>
