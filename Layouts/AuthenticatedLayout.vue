<script setup>
import { ref } from 'vue'
import ApplicationLogo from '@/Components/ApplicationLogo.vue'
import Dropdown from '@/Components/Dropdown.vue'
import DropdownLink from '@/Components/DropdownLink.vue'
import NavLink from '@/Components/NavLink.vue'
import ResponsiveNavLink from '@/Components/ResponsiveNavLink.vue'
import { Link } from '@inertiajs/vue3'

const showingNavigationDropdown = ref(false)
</script>

<template>
  <!-- Logo -->
  <Link :href="route('dashboard')">
    <ApplicationLogo />
  </Link>

  <!-- Navigation Links -->
  <NavLink :href="route('dashboard')" :active="route().current('dashboard')"> Dashboard </NavLink>

  <!-- Settings Dropdown -->
  <Dropdown align="right" width="48">
    <template #trigger>
      <button type="button">
        {{ $page.props.auth.user.name }}

        <svg
          class="ml-2 -mr-0.5 h-4 w-4"
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 20 20"
          fill="currentColor"
        >
          <path
            fill-rule="evenodd"
            d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z"
            clip-rule="evenodd"
          />
        </svg>
      </button>
    </template>

    <template #content>
      <DropdownLink :href="route('profile.edit')"> Profile </DropdownLink>
      <DropdownLink :href="route('logout')" method="post" as="button"> Log Out </DropdownLink>
    </template>
  </Dropdown>

  <!-- Primary Navigation Menu -->
  <!-- Logo -->
  <Link :href="route('dashboard')">
    <ApplicationLogo />
  </Link>

  <!-- Navigation Links -->
  <NavLink :href="route('dashboard')" :active="route().current('dashboard')"> Dashboard </NavLink>

  <!-- Hamburger -->
  <button @click="showingNavigationDropdown = !showingNavigationDropdown">
    <svg class="h-6 w-6" stroke="currentColor" fill="none" viewBox="0 0 24 24">
      <path
        :class="{
          hidden: showingNavigationDropdown,
          'inline-flex': !showingNavigationDropdown
        }"
        stroke-linecap="round"
        stroke-linejoin="round"
        stroke-width="2"
        d="M4 6h16M4 12h16M4 18h16"
      />
      <path
        :class="{
          hidden: !showingNavigationDropdown,
          'inline-flex': showingNavigationDropdown
        }"
        stroke-linecap="round"
        stroke-linejoin="round"
        stroke-width="2"
        d="M6 18L18 6M6 6l12 12"
      />
    </svg>
  </button>

  <!-- Responsive Navigation Menu -->
  <ResponsiveNavLink :href="route('dashboard')" :active="route().current('dashboard')">
    Dashboard
  </ResponsiveNavLink>

  <!-- Responsive Settings Options -->
  <ResponsiveNavLink :href="route('profile.edit')"> Profile </ResponsiveNavLink>
  <ResponsiveNavLink :href="route('logout')" method="post" as="button"> Log Out </ResponsiveNavLink>

  <!-- Page Heading -->
  <header v-if="$slots.header">
    <div>
      <slot name="header" />
    </div>
  </header>

  <!-- Page Content -->
  <main>
    <slot />
  </main>
</template>
