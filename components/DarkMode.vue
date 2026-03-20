<script setup lang="ts">
import { useStorage } from '@vueuse/core'

const mode = useStorage<'light' | 'dark'>('mode', 'light')
const isDark = computed(() => mode.value === 'dark')

useHead(() => ({
  htmlAttrs: {
    class: isDark.value ? 'dark' : '',
  },
}))

watchEffect(() => {
  if (!import.meta.client)
    return

  document.documentElement.classList.toggle('dark', isDark.value)
})

function toggleDark() {
  mode.value = isDark.value ? 'light' : 'dark'
}

function toggleViewTransition(event: MouseEvent) {
  const x = event.clientX
  const y = event.clientY
  const endRadius = Math.hypot(
    Math.max(x, innerWidth - x),
    Math.max(y, innerHeight - y),
  )
  const clipPath = [
    `circle(0px at ${x}px ${y}px)`,
    `circle(${endRadius}px at ${x}px ${y}px)`,
  ]
  const nextIsDark = !isDark.value
  // @ts-expect-error: Transition API
  const transition = document.startViewTransition(async () => {
    toggleDark()
    await nextTick()
  })

  transition.ready.then(() => {
    document.documentElement.animate(
      {
        clipPath: nextIsDark ? clipPath : [...clipPath].reverse(),
      },
      {
        duration: 300,
        easing: 'ease-in',
        pseudoElement: nextIsDark
          ? '::view-transition-new(root)'
          : '::view-transition-old(root)',
      },
    )
  })
}

function toogleTheme(event: MouseEvent) {
  const isSupport = document.startViewTransition
    && !window.matchMedia('(prefers-reduced-motion: reduce)').matches

  if (!isSupport) {
    toggleDark()
    return
  }
  toggleViewTransition(event)
}
</script>

<template>
  <div
    title="Toggle Color Scheme" class="dark:i-icon-park-outline-moon i-icon-park-outline-sun hover"
    @click="toogleTheme"
  />
</template>
