<script setup>
import logo from '@/assets/logo.svg'
import { RouterLink, useRoute } from 'vue-router';

const isActivLink =(routePath) => {
  const route = useRoute()
  return route.path === routePath
}
</script>

<template>
    <nav class="bg-green-700 border-b border-green-500">
      <div class="mx-auto max-w-7xl px-2 sm:px-6 lg:px-8">
        <div class="flex h-20 items-center justify-between">
          <div
            class="flex flex-1 items-center justify-center md:items-stretch md:justify-start"
          >
            <!-- Logo -->
            <a class="flex flex-shrink-0 items-center mr-4" href="index.html">
              <img class="h-10 w-auto" :src="logo" alt="Vue Jobs" />
              <span class="hidden md:block text-white text-2xl font-bold ml-2"
                >Vue Jobs</span
              >
            </a>
            <div class="md:ml-auto">
              <div class="flex space-x-2">
                <RouterLink
                  to="/"
                  :class="[
                    isActivLink('/')
                    ? 'bg-green-900 font-bold'
                    : 'hover:bg-gray-900 hover:text-white',
                    'text-white',
                    'px-3',
                    'py-2',
                    'rounded-md'

                  ]"
                  >Home</RouterLink
                >
                <RouterLink
                  to="/jobs"
                  :class="[
                    isActivLink('/jobs')
                    ? 'bg-green-900 font-bold'
                    : 'hover:bg-gray-900 hover:text-white',
                    'text-white',
                    'px-3',
                    'py-2',
                    'rounded-md'

                  ]"
                  >Jobs</RouterLink
                >
                <RouterLink
                  to="/jobs/add"
                  :class="[
                    isActivLink('/jobs/add')
                    ? 'bg-green-900 font-bold'
                    : 'hover:bg-gray-900 hover:text-white',
                    'text-white',
                    'px-3',
                    'py-2',
                    'rounded-md'

                  ]"
                  >Add Job</RouterLink
                >
              </div>
            </div>
          </div>
        </div>
      </div>
    </nav>
</template>