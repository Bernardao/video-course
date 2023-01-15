<template>
  <div class="main-course">
    <div class="flex mb-4 justify-between items-center w-full">
      <h1 class="text-3xl">
        <span class="font-medium">
          <span class="font-bold">{{ title }}</span>
        </span>
      </h1>
      <UserCard />
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
        <NuxtErrorBoundary>
          <NuxtPage />
          <template #error="{ error }">
            <p class="course-error">
              Oh no, something went wrong with the lesson
            </p>
            <code>{{ error }}</code>
            <p>
              <button
                class="hover:cursor-pointer bg-gray-500 text-white font-bold"
                @click="resetError(error)"
              >
                Reset
              </button>
            </p>
          </template>
        </NuxtErrorBoundary>
      </div>
    </div>
  </div>
</template>
<script lang="ts" setup>
const { chapters, title } = useCourse()
const resetError = async (error) => {
  await navigateTo(
    '/course/chapter/1-chapter-1/lesson/1-introduction-to-typescript-with-vue-js-3'
  )
  error.value = null
}
definePageMeta({
  layout: 'custom',
  // layout: false
})
</script>

<style scoped>
.router-link-active {
  @apply text-blue-500;
}
</style>
