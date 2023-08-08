<script setup lang="ts">
import Calendar from "./components/Calendar.vue";
import Stats from "./components/Stats.vue";
</script>

<template>
  <main class="mx-auto max-w-7xl px-4 py-10 sm:px-6 lg:px-8">
    <div class="mx-auto max-w-3xl min-h-screen space-y-8">
      <div class="flex justify-between">
        <h1 class="text-2xl font-extrabold text-center sm:text-left">
          16 Weeks
        </h1>

        <a href="https://vitejs.dev" target="_blank">
          <img src="/vite.svg" alt="Vite logo" />
        </a>
      </div>

      <div
        class="divide-y divide-zinc-600 overflow-hidden rounded-lg bg-zinc-900"
      >
        <div class="px-4 py-5 sm:px-6">
          <Stats />
        </div>
        <div class="px-4 py-5 sm:p-6">
          <Calendar />
        </div>
      </div>
    </div>
    <div class="flex justify-center py-6">
      <a
        href="https://www.youtube.com/watch?v=P1q7t07CW_E"
        target="_blank"
        rel="noopener noreferrer"
      >
        <img src="/bear.png" alt="Bear down for midterms" />
      </a>
    </div>
  </main>
</template>
