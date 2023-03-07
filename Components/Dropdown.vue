<script setup>
import { computed, onMounted, onUnmounted, ref } from 'vue'

const props = defineProps({
  align: {
    default: 'right'
  },
  width: {
    default: '48'
  },
  contentClasses: {
    default: () => []
  }
})

const closeOnEscape = (e) => {
  if (open.value && e.key === 'Escape') {
    open.value = false
  }
}

onMounted(() => document.addEventListener('keydown', closeOnEscape))
onUnmounted(() => document.removeEventListener('keydown', closeOnEscape))

const widthClass = computed(() => {
  return {
    48: '48px'
  }[props.width.toString()]
})

const alignmentClasses = computed(() => {
  if (props.align === 'left') {
    return 'align-left'
  } else if (props.align === 'right') {
    return 'align-right'
  } else {
    return ''
  }
})

const open = ref(false)
</script>

<template>
  <div class="relative">
    <div @click="open = !open">
      <slot name="trigger" />
    </div>

    <!-- Full Screen Dropdown Overlay -->
    <div v-show="open" @click="open = false"></div>

    <!-- <transition
            enter-active-class="transition ease-out duration-200"
            enter-from-class="transform opacity-0 scale-95"
            enter-to-class="transform opacity-100 scale-100"
            leave-active-class="transition ease-in duration-75"
            leave-from-class="transform opacity-100 scale-100"
            leave-to-class="transform opacity-0 scale-95"
        > -->
    <transition>
      <div
        v-show="open"
        :class="[widthClass, alignmentClasses]"
        style="display: none"
        @click="open = false"
      >
        <div :class="contentClasses">
          <slot name="content" />
        </div>
      </div>
    </transition>
  </div>
</template>
