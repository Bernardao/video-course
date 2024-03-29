<script setup lang="ts">
import { Course } from '~~/types'

const course: Course = useCourse()
const route = useRoute()

definePageMeta({
  middleware: [
    function ({ params }, from) {
      const course: Course = useCourse()
      const chapter = course?.chapters?.find(
        (chapter) => chapter.slug === params.chapterSlug
      )
      if (!chapter) {
        return abortNavigation(
          createError({
            statusCode: 404,
            message: 'Chapter not found',
          })
        )
      }
      const lesson = chapter?.lessons?.find(
        (lesson) => lesson.slug === params.lessonSlug
      )
      if (!lesson) {
        return abortNavigation(
          createError({
            statusCode: 404,
            message: 'Lesson not found',
          })
        )
      }
    },
    'auth-manual',
  ],
})

// if (route.params.lessonSlug === '3-typing-component-events') {
//   console.log(route.params.paramthatdoesnotexits.capitalizeIsNotAMethod())
// }

const progress = useLocalStorage('progress', [])

const chapter = computed(() => {
  return course.chapters?.find(
    (chapter) => chapter.slug === route.params.chapterSlug
  )
})

const lesson = computed(() => {
  return chapter.value?.lessons?.find(
    (lesson) => lesson.slug === route.params.lessonSlug
  )
})

const title = computed((): string => {
  return `MasteringNuxt: ${lesson.value?.title} - ${course.title}`
})
useHead({
  title,
})

const isLessonComplete = computed((): boolean => {
  if (chapter.value && lesson.value) {
    if (!progress.value[chapter.value.number - 1]) {
      return false
    }

    if (!progress.value[chapter.value.number - 1][lesson.value.number - 1]) {
      return false
    }

    return progress.value[chapter.value.number - 1][lesson.value.number - 1]
  }
  return false
})

const toggleComplete = () => {
  throw createError('Could not update')
  if (!progress.value[chapter.value?.number - 1]) {
    progress.value[chapter.value.number - 1] = []
  }

  progress.value[chapter.value.number - 1][lesson.value.number - 1] =
    !isLessonComplete.value
}
</script>
<template>
  <article v-if="lesson && chapter" class="lessons">
    <p class="mt-0 uppercase font-bold text-slate-400 mb-1">
      Lesson {{ chapter.number }} - {{ lesson.number }}
    </p>
    <h2 class="my-0">{{ lesson.title }}</h2>
    <div class="flex space-x-4 mt-2 mb-8">
      <NuxtLink
        v-if="lesson.sourceUrl"
        :to="lesson.sourceUrl"
        class="font-normal text-md text-gray-500"
      >
        Download Source Code
      </NuxtLink>
      <NuxtLink
        v-if="lesson.downloadUrl"
        :to="lesson.downloadUrl"
        class="font-normal text-md text-gray-500"
      >
        Download Video
      </NuxtLink>
    </div>
    <VideoPlayer v-if="lesson.videoId" :videoId="lesson.videoId" />
    <p>{{ lesson.text }}</p>
    <ClientOnly>
      <LessonCompleteButton
        :model-value="isLessonComplete"
        @update:model-value="toggleComplete"
      />
    </ClientOnly>
  </article>
</template>

<style scoped></style>
