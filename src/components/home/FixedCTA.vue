<script setup lang="ts">
import { ref, watch, onUnmounted } from 'vue'

const props = defineProps<{
  observeTarget: HTMLElement | undefined
}>()

const visible = ref(false)
let observer: IntersectionObserver | null = null

watch(
  () => props.observeTarget,
  (target) => {
    observer?.disconnect()
    if (!target) return

    observer = new IntersectionObserver(
      (entries) => {
        const entry = entries[0]
        if (entry) visible.value = !entry.isIntersecting
      },
      { threshold: 0 },
    )
    observer.observe(target)
  },
  { immediate: true },
)

onUnmounted(() => {
  observer?.disconnect()
})
</script>

<template>
  <Transition name="fixed-cta">
    <a
      v-show="visible"
      href="#cach-tham-gia"
      class="fixed bottom-6 left-1/2 z-40 -translate-x-1/2 border border-accent-coral bg-accent-coral/10 px-6 py-2.5 font-display font-semibold text-accent-coral tracking-wide backdrop-blur-sm transition-all duration-300 hover:bg-accent-coral hover:text-bg-deep"
    >
      Tham gia ngay
    </a>
  </Transition>
</template>

<style scoped>
.fixed-cta-enter-active,
.fixed-cta-leave-active {
  transition:
    opacity 0.3s ease,
    transform 0.3s ease;
}

.fixed-cta-enter-from,
.fixed-cta-leave-to {
  opacity: 0;
  transform: translate(-50%, 12px);
}
</style>
