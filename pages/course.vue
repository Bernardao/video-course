<script lang="ts" setup>
const { chapters } = useCourse()
</script>
<template>
  <div
    class="p-12 bg-red-100 w-full h-full min-h-screen flex flex-col items-center"
  >
    <div class="prose mb-12">
      <h1>
        <span class="font-medium">
          Course:
          <span class="font-bold"> Mastering Nuxt </span>
        </span>
      </h1>
    </div>
    <div class="flex flex-row justify-center flex-grow">
      <aside
        class="prose mr-4 p-8 bg-white rounded-md min-w-[20ch] max-w-[30ch] flex flex-col"
      >
        <h3>Chapters</h3>
        <div
          v-for="chapter in chapters"
          :key="chapter.slug"
          class="space-y-1 mb-4 flex flex-col"
        >
          <h4>{{ chapter.title }}</h4>
          <NuxtLink
            v-for="(lesson, index) in chapter.lessons"
            :key="lesson.slug"
            class="flex flex-row space-x-1 no-underline prose-sm font-normal py-1 px-4 mx-4"
            :to="lesson.path"
            :class="{
              'text-blue-500': lesson.path === $route.fullPath,
              'text-gray-500': lesson.path !== $route.fullPath,
            }"
          >
            <span class="text-gray-500">{{ index + 1 }}</span>
            <span>{{ lesson.title }}</span>
          </NuxtLink>
        </div>
      </aside>
      <div class="prose p-12 bg-white-100 rounded-md w-[65ch]">
        <NuxtPage />
      </div>
    </div>
  </div>
</template>
<style scoped>
.router-link-active {
  @apply text-blue-500;
}
</style>
