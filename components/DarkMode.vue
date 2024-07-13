<script setup lang="ts">
import { useStorage} from '@vueuse/core'

let mode = useStorage('mode', 'light')
let isDark = mode.value === 'dark'

function toggleDark() {
  const root = document.documentElement
  isDark = root.classList.contains('dark')
  root.classList.remove(isDark ? 'dark' : '-')
  root.classList.add(isDark ? '-' : 'dark')
  mode.value = isDark ? 'light' : 'dark'
}

function setModeClass(isDark: boolean): void {
  if (isDark) {
    useHead({
      htmlAttrs: { class: 'dark' },
    })
  } else {
    useHead({
      htmlAttrs: { class: '' },
    })
  }
}

watchEffect(() => {
  if (isDark) {
    setModeClass(true)
  } else {
    setModeClass(false)
  }
})

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
  // @ts-expect-error: Transition API
  const transition = document.startViewTransition(async () => {
    toggleDark()
    await nextTick()
  })

  transition.ready.then(() => {
    document.documentElement.animate(
      {
        clipPath: isDark ? [...clipPath].reverse() : clipPath,
      },
      {
        duration: 300,
        easing: 'ease-in',
        pseudoElement: isDark
          ? '::view-transition-old(root)'
          : '::view-transition-new(root)',
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
  <div title="Toggle Color Scheme" class="dark:i-icon-park-outline-moon i-icon-park-outline-sun hover"
    @click="toogleTheme" />
</template>
