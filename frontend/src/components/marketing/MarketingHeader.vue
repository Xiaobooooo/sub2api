<template>
  <header class="sticky top-0 z-40 border-b border-gray-200/80 bg-white/85 backdrop-blur-xl dark:border-dark-800 dark:bg-dark-950/85">
    <nav class="mx-auto flex h-16 max-w-7xl items-center justify-between px-4 sm:px-6 lg:px-8">
      <router-link
        to="/home"
        class="flex min-w-0 items-center gap-3 text-gray-900 dark:text-white"
        @click="mobileOpen = false"
      >
        <span class="sidebar-logo flex h-9 w-9 shrink-0 items-center justify-center overflow-hidden rounded-xl shadow-glow">
          <img :src="logoSrc" alt="Logo" class="h-full w-full object-contain" />
        </span>
        <span class="truncate text-xl font-bold tracking-tight">
          {{ siteName }}
        </span>
      </router-link>

      <div class="hidden items-center gap-1 lg:flex">
        <router-link
          v-for="item in navItems"
          :key="item.label"
          :to="item.to"
          class="rounded-xl px-3 py-2 text-sm font-medium transition-colors"
          :class="isActive(item.path)
            ? 'bg-primary-50 text-primary-700 dark:bg-primary-900/20 dark:text-primary-300'
            : 'text-gray-600 hover:bg-gray-100 hover:text-gray-900 dark:text-dark-300 dark:hover:bg-dark-800 dark:hover:text-white'"
        >
          {{ item.label }}
        </router-link>
      </div>

      <div class="hidden items-center gap-2 lg:flex">
        <LocaleSwitcher />
        <button
          type="button"
          class="btn btn-ghost btn-icon"
          :title="isDark ? copy.switchToLight : copy.switchToDark"
          @click="toggleTheme"
        >
          <Icon v-if="isDark" name="sun" size="md" />
          <Icon v-else name="moon" size="md" />
        </button>
        <router-link
          :to="isAuthenticated ? dashboardPath : '/login'"
          class="btn btn-primary btn-md"
        >
          {{ isAuthenticated ? copy.dashboard : copy.login }}
          <Icon name="arrowRight" size="sm" />
        </router-link>
      </div>

      <button
        type="button"
        class="btn btn-ghost btn-icon lg:hidden"
        :aria-label="copy.openNavigation"
        @click="mobileOpen = !mobileOpen"
      >
        <Icon :name="mobileOpen ? 'x' : 'menu'" size="md" />
      </button>
    </nav>

    <div
      v-if="mobileOpen"
      class="border-t border-gray-200 bg-white px-4 py-4 shadow-lg dark:border-dark-800 dark:bg-dark-950 lg:hidden"
    >
      <div class="mx-auto flex max-w-7xl flex-col gap-2">
        <router-link
          v-for="item in navItems"
          :key="item.label"
          :to="item.to"
          class="rounded-xl px-3 py-3 text-sm font-medium transition-colors"
          :class="isActive(item.path)
            ? 'bg-primary-50 text-primary-700 dark:bg-primary-900/20 dark:text-primary-300'
            : 'text-gray-600 hover:bg-gray-100 hover:text-gray-900 dark:text-dark-300 dark:hover:bg-dark-800 dark:hover:text-white'"
          @click="mobileOpen = false"
        >
          {{ item.label }}
        </router-link>
        <div class="mt-3 flex items-center justify-between gap-3 border-t border-gray-100 pt-4 dark:border-dark-800">
          <LocaleSwitcher />
          <button
            type="button"
            class="btn btn-ghost btn-icon"
            :title="isDark ? copy.switchToLight : copy.switchToDark"
            @click="toggleTheme"
          >
            <Icon v-if="isDark" name="sun" size="md" />
            <Icon v-else name="moon" size="md" />
          </button>
          <router-link
            :to="isAuthenticated ? dashboardPath : '/login'"
            class="btn btn-primary btn-md"
            @click="mobileOpen = false"
          >
            {{ isAuthenticated ? copy.dashboard : copy.login }}
          </router-link>
        </div>
      </div>
    </div>
  </header>
</template>

<script setup lang="ts">
import { computed, onMounted, ref } from 'vue'
import { useI18n } from 'vue-i18n'
import { useRoute } from 'vue-router'
import LocaleSwitcher from '@/components/common/LocaleSwitcher.vue'
import Icon from '@/components/icons/Icon.vue'
import { useAppStore, useAuthStore } from '@/stores'

const route = useRoute()
const { locale } = useI18n()
const appStore = useAppStore()
const authStore = useAuthStore()
const mobileOpen = ref(false)
const isDark = ref(document.documentElement.classList.contains('dark'))

const headerCopy = {
  zh: {
    nav: {
      home: '首页',
      pricing: '模型价格',
      docs: '接入文档'
    },
    dashboard: '控制台',
    login: '登录',
    openNavigation: '打开导航',
    switchToLight: '切换到浅色模式',
    switchToDark: '切换到深色模式'
  },
  en: {
    nav: {
      home: 'Home',
      pricing: 'Pricing',
      docs: 'Docs'
    },
    dashboard: 'Dashboard',
    login: 'Login',
    openNavigation: 'Open navigation',
    switchToLight: 'Switch to light mode',
    switchToDark: 'Switch to dark mode'
  }
} as const

const activeLocale = computed(() => locale.value === 'zh' ? 'zh' : 'en')
const copy = computed(() => headerCopy[activeLocale.value])

const navItems = computed(() => [
  { label: copy.value.nav.home, to: '/home', path: '/home' },
  { label: copy.value.nav.pricing, to: '/pricing', path: '/pricing' },
  { label: copy.value.nav.docs, to: '/docs', path: '/docs' }
])

const siteName = computed(() => appStore.cachedPublicSettings?.site_name || appStore.siteName || 'Sub2API')
const siteLogo = computed(() => appStore.cachedPublicSettings?.site_logo || appStore.siteLogo || '')
const logoSrc = computed(() => siteLogo.value || '/logo.png')
const isAuthenticated = computed(() => authStore.isAuthenticated)
const dashboardPath = computed(() => authStore.isAdmin ? '/admin/dashboard' : '/dashboard')

function isActive(path: string): boolean {
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
