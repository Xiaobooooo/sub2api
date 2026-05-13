<template>
  <header class="sticky top-0 z-40 border-b border-white/10 bg-[#171713]/90 backdrop-blur-xl">
    <nav class="mx-auto flex h-20 max-w-7xl items-center justify-between px-5 sm:px-8">
      <router-link
        to="/home"
        class="flex min-w-0 items-center gap-3 text-[#faf9f5]"
        @click="mobileOpen = false"
      >
        <span
          class="flex h-10 w-10 shrink-0 items-center justify-center overflow-hidden rounded-lg border border-primary-300/25 bg-[#252320]"
        >
          <img
            v-if="siteLogo"
            :src="siteLogo"
            alt="Logo"
            class="h-full w-full object-contain"
          />
          <Icon v-else name="sparkles" size="md" class="text-primary-300" />
        </span>
        <span class="truncate font-serif text-2xl font-semibold text-primary-200">
          {{ siteName }}
        </span>
      </router-link>

      <div class="hidden items-center gap-8 lg:flex">
        <router-link
          v-for="item in navItems"
          :key="item.label"
          :to="item.to"
          class="text-sm font-medium transition-colors"
          :class="isActive(item.path) ? 'text-primary-200' : 'text-[#d7d2c8] hover:text-white'"
        >
          {{ item.label }}
        </router-link>
      </div>

      <div class="hidden items-center gap-3 lg:flex">
        <LocaleSwitcher />
        <button
          type="button"
          class="flex h-10 w-10 items-center justify-center rounded-lg text-[#d7d2c8] transition-colors hover:bg-white/10 hover:text-white"
          :title="isDark ? '切换到浅色模式' : '切换到深色模式'"
          @click="toggleTheme"
        >
          <Icon v-if="isDark" name="sun" size="md" />
          <Icon v-else name="moon" size="md" />
        </button>
        <router-link
          :to="isAuthenticated ? dashboardPath : '/login'"
          class="inline-flex h-11 items-center gap-2 rounded-lg bg-primary-600 px-5 text-sm font-semibold text-white transition-colors hover:bg-primary-700"
        >
          {{ isAuthenticated ? '控制台' : '登录' }}
          <Icon name="arrowRight" size="sm" />
        </router-link>
      </div>

      <button
        type="button"
        class="flex h-10 w-10 items-center justify-center rounded-lg text-[#faf9f5] transition-colors hover:bg-white/10 lg:hidden"
        aria-label="打开导航"
        @click="mobileOpen = !mobileOpen"
      >
        <Icon :name="mobileOpen ? 'x' : 'menu'" size="md" />
      </button>
    </nav>

    <div
      v-if="mobileOpen"
      class="border-t border-white/10 bg-[#171713] px-5 py-4 shadow-2xl lg:hidden"
    >
      <div class="mx-auto flex max-w-7xl flex-col gap-2">
        <router-link
          v-for="item in navItems"
          :key="item.label"
          :to="item.to"
          class="rounded-lg px-3 py-3 text-sm font-medium transition-colors"
          :class="isActive(item.path) ? 'bg-primary-600/20 text-primary-200' : 'text-[#d7d2c8] hover:bg-white/10 hover:text-white'"
          @click="mobileOpen = false"
        >
          {{ item.label }}
        </router-link>
        <div class="mt-3 flex items-center justify-between gap-3 border-t border-white/10 pt-4">
          <LocaleSwitcher />
          <button
            type="button"
            class="flex h-10 w-10 items-center justify-center rounded-lg text-[#d7d2c8] transition-colors hover:bg-white/10 hover:text-white"
            :title="isDark ? '切换到浅色模式' : '切换到深色模式'"
            @click="toggleTheme"
          >
            <Icon v-if="isDark" name="sun" size="md" />
            <Icon v-else name="moon" size="md" />
          </button>
          <router-link
            :to="isAuthenticated ? dashboardPath : '/login'"
            class="inline-flex h-10 items-center justify-center rounded-lg bg-primary-600 px-4 text-sm font-semibold text-white transition-colors hover:bg-primary-700"
            @click="mobileOpen = false"
          >
            {{ isAuthenticated ? '控制台' : '登录' }}
          </router-link>
        </div>
      </div>
    </div>
  </header>
</template>

<script setup lang="ts">
import { computed, onMounted, ref } from 'vue'
import { useRoute } from 'vue-router'
import LocaleSwitcher from '@/components/common/LocaleSwitcher.vue'
import Icon from '@/components/icons/Icon.vue'
import { useAppStore, useAuthStore } from '@/stores'

const route = useRoute()
const appStore = useAppStore()
const authStore = useAuthStore()
const mobileOpen = ref(false)
const isDark = ref(document.documentElement.classList.contains('dark'))

const navItems = [
  { label: '首页', to: '/home', path: '/home' },
  { label: '模型价格', to: '/pricing', path: '/pricing' },
  { label: '接入文档', to: '/docs', path: '/docs' },
  { label: '常见问题', to: { path: '/home', hash: '#faq' }, path: '/home#faq' }
] as const

const siteName = computed(() => appStore.cachedPublicSettings?.site_name || appStore.siteName || 'Sub2API')
const siteLogo = computed(() => appStore.cachedPublicSettings?.site_logo || appStore.siteLogo || '')
const isAuthenticated = computed(() => authStore.isAuthenticated)
const dashboardPath = computed(() => authStore.isAdmin ? '/admin/dashboard' : '/dashboard')

function isActive(path: string): boolean {
  if (path === '/home#faq') {
    return route.path === '/home' && route.hash === '#faq'
  }
  return route.path === path
}

function toggleTheme(): void {
  isDark.value = !isDark.value
  document.documentElement.classList.toggle('dark', isDark.value)
  localStorage.setItem('theme', isDark.value ? 'dark' : 'light')
}

function initTheme(): void {
  const savedTheme = localStorage.getItem('theme')
  if (
    savedTheme === 'dark' ||
    (!savedTheme && window.matchMedia('(prefers-color-scheme: dark)').matches)
  ) {
    isDark.value = true
    document.documentElement.classList.add('dark')
  }
}

onMounted(() => {
  initTheme()
  authStore.checkAuth()
  if (!appStore.publicSettingsLoaded) {
    appStore.fetchPublicSettings()
  }
})
</script>
