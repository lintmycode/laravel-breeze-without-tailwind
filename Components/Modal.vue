<script setup>
import { computed, onMounted, onUnmounted, watch } from 'vue'

const props = defineProps({
  show: {
    type: Boolean,
    default: false
  },
  maxWidth: {
    type: String
    // default: '2xl',
  },
  closeable: {
    type: Boolean,
    default: true
  }
})

const emit = defineEmits(['close'])

watch(
  () => props.show,
  () => {
    if (props.show) {
      document.body.style.overflow = 'hidden'
    } else {
      document.body.style.overflow = null
    }
  }
)

const close = () => {
  if (props.closeable) {
    emit('close')
  }
}

const closeOnEscape = (e) => {
  if (e.key === 'Escape' && props.show) {
    close()
  }
}

onMounted(() => document.addEventListener('keydown', closeOnEscape))

onUnmounted(() => {
  document.removeEventListener('keydown', closeOnEscape)
  document.body.style.overflow = null
})

const maxWidthClass = computed(() => {
  return {
    sm: 'sm',
    md: 'md',
    lg: 'lg',
    xl: 'xl'
  }[props.maxWidth]
})
</script>

<template>
  <teleport to="body">
    <transition>
      <div v-show="show" scroll-region>
        <transition>
          <div v-show="show" @click="close">
            <!-- <div class="absolute inset-0 bg-gray-500 opacity-75" /> -->
            <div />
          </div>
        </transition>

        <transition>
          <div v-show="show" :class="maxWidthClass">
            <slot v-if="show" />
          </div>
        </transition>
      </div>
    </transition>
  </teleport>
</template>
