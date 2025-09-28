<script setup lang="ts">
import { ref } from 'vue'
import {
    Dialog,
    DialogPanel,
    Menu,
    MenuButton,
    MenuItem,
    MenuItems,
    TransitionChild,
    TransitionRoot,
} from '@headlessui/vue'
import {
    Bars3Icon,
    BellIcon,
    CalendarIcon,
    ChartPieIcon,
    Cog6ToothIcon,
    DocumentDuplicateIcon,
    FolderIcon,
    HomeIcon,
    UsersIcon,
    XMarkIcon,
} from '@heroicons/vue/24/outline'
import { ChevronDownIcon, MagnifyingGlassIcon } from '@heroicons/vue/20/solid'

const navigation = [
    { name: 'Dashboard', href: '#', icon: HomeIcon, current: true },
    { name: 'Team', href: '#', icon: UsersIcon, current: false },
    { name: 'Projects', href: '#', icon: FolderIcon, current: false },
    { name: 'Calendar', href: '#', icon: CalendarIcon, current: false },
    { name: 'Documents', href: '#', icon: DocumentDuplicateIcon, current: false },
    { name: 'Reports', href: '#', icon: ChartPieIcon, current: false },
]
const teams = [
    { id: 1, name: 'Heroicons', href: '#', initial: 'H', current: false },
    { id: 2, name: 'Tailwind Labs', href: '#', initial: 'T', current: false },
    { id: 3, name: 'Workcation', href: '#', initial: 'W', current: false },
]
const userNavigation = [
    { name: 'Your profile', href: '#' },
    { name: 'Sign out', href: '#' },
]

const sidebarOpen = ref(false)
// Touch interactions for bottom sheet handle and sheet
const startY = ref<number | null>(null)
const lastY = ref<number | null>(null)
const onHandleTouchStart = (e: TouchEvent) => {
  startY.value = e.touches[0].clientY
  lastY.value = startY.value
}
const onHandleTouchMove = (e: TouchEvent) => {
  lastY.value = e.touches[0].clientY
}
const onHandleTouchEnd = () => {
  if (startY.value != null && lastY.value != null) {
    const delta = startY.value - lastY.value
    if (delta > 30) sidebarOpen.value = true
  }
  startY.value = null
  lastY.value = null
}

const sheetStartY = ref<number | null>(null)
const sheetLastY = ref<number | null>(null)
const onSheetTouchStart = (e: TouchEvent) => {
  sheetStartY.value = e.touches[0].clientY
  sheetLastY.value = sheetStartY.value
}
const onSheetTouchMove = (e: TouchEvent) => {
  sheetLastY.value = e.touches[0].clientY
}
const onSheetTouchEnd = () => {
  if (sheetStartY.value != null && sheetLastY.value != null) {
    const delta = sheetStartY.value - sheetLastY.value
    if (delta < -30) sidebarOpen.value = false
  }
  sheetStartY.value = null
  sheetLastY.value = null
}
</script>
<template>
  <!--
    This example requires updating your template:

    ```
    <html class="h-full bg-white dark:bg-gray-900">
    <body class="h-full">
    ```
  -->
  <div>
    <!-- Bottom sheet for mobile -->
    <TransitionRoot as="template" :show="sidebarOpen">
      <Dialog class="relative z-50 lg:hidden" @close="sidebarOpen = false">
        <TransitionChild as="template" enter="transition-opacity ease-linear duration-200" enter-from="opacity-0" enter-to="" leave="transition-opacity ease-linear duration-200" leave-from="" leave-to="opacity-0">
          <div class="fixed inset-0 bg-gray-900/40" />
        </TransitionChild>

        <div class="fixed inset-x-0 bottom-0 flex justify-center">
          <TransitionChild as="template" enter="transition ease-in-out duration-300 transform" enter-from="translate-y-full" enter-to="translate-y-0" leave="transition ease-in-out duration-300 transform" leave-from="translate-y-0" leave-to="translate-y-full">
            <DialogPanel class="w-full max-w-lg sm:max-w-xl md:max-w-2xl rounded-t-2xl bg-white dark:bg-gray-900 shadow-lg ring-1 ring-gray-200 dark:ring-white/10" @touchstart="onSheetTouchStart" @touchmove.prevent="onSheetTouchMove" @touchend="onSheetTouchEnd">
              <div class="mx-auto mt-3 mb-2 h-1.5 w-12 rounded-full bg-gray-300 dark:bg-white/20" />
              <div class="px-4 pb-4">
                <div class="grid grid-cols-2 gap-3">
                  <a v-for="item in navigation" :key="item.name" :href="item.href" class="group rounded-xl ring-1 ring-gray-200 dark:ring-white/10 bg-gray-50 dark:bg-white/5 p-4 flex items-center gap-3">
                    <component :is="item.icon" class="size-6 text-gray-400 group-hover:text-indigo-600 dark:text-gray-300 dark:group-hover:text-white" aria-hidden="true" />
                    <span class="text-sm font-semibold text-gray-900 dark:text-white">{{ item.name }}</span>
                  </a>
                </div>
                <div class="mt-4">
                  <div class="text-xs font-semibold text-gray-400">Your teams</div>
                  <div class="mt-2 grid grid-cols-2 gap-3">
                    <a v-for="team in teams" :key="team.name" :href="team.href" class="group rounded-xl ring-1 ring-gray-200 dark:ring-white/10 bg-gray-50 dark:bg-white/5 p-4 flex items-center gap-3">
                      <span class="flex size-6 shrink-0 items-center justify-center rounded-lg border bg-white text-[0.625rem] font-medium border-gray-200 text-gray-400 dark:bg-white/5 dark:border-white/10">{{ team.initial }}</span>
                      <span class="truncate text-sm font-semibold text-gray-900 dark:text-white">{{ team.name }}</span>
                    </a>
                  </div>
                </div>
              </div>
            </DialogPanel>
          </TransitionChild>
        </div>
      </Dialog>
    </TransitionRoot>

    <!-- Persistent bottom handle when sheet is closed -->
    <div class="fixed inset-x-0 bottom-0 z-40 lg:hidden flex justify-center pb-2" @click="sidebarOpen = true" @touchstart="onHandleTouchStart" @touchmove.prevent="onHandleTouchMove" @touchend="onHandleTouchEnd">
      <div class="h-8 -translate-y-1 flex items-center">
        <div class="h-1.5 w-12 rounded-full bg-gray-300 dark:bg-white/20" />
      </div>
    </div>

    <!-- Static sidebar for desktop -->
    <div class="hidden lg:fixed lg:inset-y-0 lg:z-50 lg:flex lg:w-72 lg:flex-col dark:bg-gray-900">
      <!-- Sidebar component, swap this element with another sidebar if you like -->
      <div class="flex grow flex-col gap-y-5 overflow-y-auto border-r border-gray-200 bg-white px-6 pb-4 dark:border-white/10 dark:bg-black/10">
        <div class="flex h-16 shrink-0 items-center">
          <img class="h-8 w-auto dark:hidden" src="/plus-assets/img/logos/mark.svg?color=indigo&shade=600" alt="Your Company" />
          <img class="h-8 w-auto not-dark:hidden" src="/plus-assets/img/logos/mark.svg?color=indigo&shade=500" alt="Your Company" />
        </div>
        <nav class="flex flex-1 flex-col">
          <ul role="list" class="flex flex-1 flex-col gap-y-7">
            <li>
              <ul role="list" class="-mx-2 space-y-1">
                <li v-for="item in navigation" :key="item.name">
                  <a :href="item.href" :class="[item.current ? 'bg-gray-50 text-indigo-600 dark:bg-white/5 dark:text-white' : 'text-gray-700 hover:bg-gray-50 hover:text-indigo-600 dark:text-gray-400 dark:hover:bg-white/5 dark:hover:text-white', 'group flex gap-x-3 rounded-md p-2 text-sm/6 font-semibold']">
                    <component :is="item.icon" :class="[item.current ? 'text-indigo-600 dark:text-white' : 'text-gray-400 group-hover:text-indigo-600 dark:group-hover:text-white', 'size-6 shrink-0']" aria-hidden="true" />
                    {{ item.name }}
                  </a>
                </li>
              </ul>
            </li>
            <li>
              <div class="text-xs/6 font-semibold text-gray-400">Your teams</div>
              <ul role="list" class="-mx-2 mt-2 space-y-1">
                <li v-for="team in teams" :key="team.name">
                  <a :href="team.href" :class="[team.current ? 'bg-gray-50 text-indigo-600 dark:bg-white/5 dark:text-white' : 'text-gray-700 hover:bg-gray-50 hover:text-indigo-600 dark:text-gray-400 dark:hover:bg-white/5 dark:hover:text-white', 'group flex gap-x-3 rounded-md p-2 text-sm/6 font-semibold']">
                    <span :class="[team.current ? 'border-indigo-600 text-indigo-600 dark:border-white/20 dark:text-white' : 'border-gray-200 text-gray-400 group-hover:border-indigo-600 group-hover:text-indigo-600 dark:border-white/10 dark:group-hover:border-white/20 dark:group-hover:text-white', 'flex size-6 shrink-0 items-center justify-center rounded-lg border bg-white text-[0.625rem] font-medium dark:bg-white/5']">{{ team.initial }}</span>
                    <span class="truncate">{{ team.name }}</span>
                  </a>
                </li>
              </ul>
            </li>
            <li class="mt-auto">
              <a href="#" class="group -mx-2 flex gap-x-3 rounded-md p-2 text-sm/6 font-semibold text-gray-700 hover:bg-gray-50 hover:text-indigo-600 dark:text-gray-400 dark:hover:bg-white/5 dark:hover:text-white">
                <Cog6ToothIcon class="size-6 shrink-0 text-gray-400 group-hover:text-indigo-600 dark:group-hover:text-white" aria-hidden="true" />
                Settings
              </a>
            </li>
          </ul>
        </nav>
      </div>
    </div>

    <div class="lg:pl-72">
      <div class="sticky top-0 z-40 lg:mx-auto lg:max-w-7xl lg:px-8">
        <div class="flex h-16 items-center gap-x-4 border-b border-gray-200 bg-white px-4 shadow-xs sm:gap-x-6 sm:px-6 lg:px-0 lg:shadow-none dark:border-white/10 dark:bg-gray-900 dark:shadow-none">
          <button type="button" class="-m-2.5 p-2.5 text-gray-700 hover:text-gray-900 lg:hidden dark:text-gray-400 dark:hover:text-white" @click="sidebarOpen = true">
            <span class="sr-only">Open sidebar</span>
            <Bars3Icon class="size-6" aria-hidden="true" />
          </button>

          <!-- Separator -->
          <div class="h-6 w-px bg-gray-200 lg:hidden dark:bg-gray-700" aria-hidden="true" />

          <div class="flex flex-1 gap-x-4 self-stretch lg:gap-x-6">
            <form class="grid flex-1 grid-cols-1" action="#" method="GET">
              <input name="search" aria-label="Search" class="col-start-1 row-start-1 block size-full bg-white pl-8 text-base text-gray-900 outline-hidden placeholder:text-gray-400 sm:text-sm/6 dark:bg-gray-900 dark:text-white dark:placeholder:text-gray-500" placeholder="Search" />
              <MagnifyingGlassIcon class="pointer-events-none col-start-1 row-start-1 size-5 self-center text-gray-400" aria-hidden="true" />
            </form>
            <div class="flex items-center gap-x-4 lg:gap-x-6">
              <button type="button" class="-m-2.5 p-2.5 text-gray-400 hover:text-gray-500 dark:hover:text-white">
                <span class="sr-only">View notifications</span>
                <BellIcon class="size-6" aria-hidden="true" />
              </button>

              <!-- Separator -->
              <div class="hidden lg:block lg:h-6 lg:w-px lg:bg-gray-200 dark:lg:bg-white/10" aria-hidden="true" />

              <!-- Profile dropdown -->
              <Menu as="div" class="relative">
                <MenuButton class="relative flex items-center">
                  <span class="absolute -inset-1.5" />
                  <span class="sr-only">Open user menu</span>
                  <img class="size-8 rounded-full bg-gray-50 outline -outline-offset-1 outline-black/5 dark:bg-gray-800 dark:outline-white/10" src="https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80" alt="" />
                  <span class="hidden lg:flex lg:items-center">
                    <span class="ml-4 text-sm/6 font-semibold text-gray-900 dark:text-white" aria-hidden="true">Tom Cook</span>
                    <ChevronDownIcon class="ml-2 size-5 text-gray-400" aria-hidden="true" />
                  </span>
                </MenuButton>
                <transition enter-active-class="transition ease-out duration-100" enter-from-class="transform opacity-0 scale-95" enter-to-class="transform scale-100" leave-active-class="transition ease-in duration-75" leave-from-class="transform scale-100" leave-to-class="transform opacity-0 scale-95">
                  <MenuItems class="absolute right-0 z-10 mt-2.5 w-32 origin-top-right rounded-md bg-white py-2 shadow-lg outline-1 outline-gray-900/5 dark:bg-gray-800 dark:shadow-none dark:-outline-offset-1 dark:outline-white/10">
                    <MenuItem v-for="item in userNavigation" :key="item.name" v-slot="{ active }">
                      <a :href="item.href" :class="[active ? 'bg-gray-50 outline-hidden dark:bg-gray-700' : '', 'block px-3 py-1 text-sm/6 text-gray-900 dark:text-white']">{{ item.name }}</a>
                    </MenuItem>
                  </MenuItems>
                </transition>
              </Menu>
            </div>
          </div>
        </div>
      </div>

      <main class="py-10">
        <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
          <!-- Your content -->
        </div>
      </main>
    </div>
  </div>
</template>
