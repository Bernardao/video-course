<template>
  <NuxtLayout>
    <main class="main-error flex flex-col">
      <div v-if="error.statusCode === '404'" class="prose">
        <h1>404</h1>
        <p>I guess that page does not exists</p>
        <strong v-if="error">{{ error.message }}</strong>
      </div>

      <div v-else class="prose">
        <h1>Dang</h1>
        <p>It looks like something broke.</p>
        <p>Sorry about that</p>
        <strong v-if="error">{{ error.message }}</strong>
      </div>
      <div class="prose mt-5">
        <p>
          Go to the
          <a class="hover:cursor-pointer" @click="handleError">
            <strong> First lesson </strong>
          </a>
        </p>
      </div>
    </main>
  </NuxtLayout>
</template>

<script setup lang="ts">
const error = useError()
const handleError = () => {
  clearError({
    redirect:
      '/course/chapter/1-chapter-1/lesson/1-introduction-to-typescript-with-vue-js-3',
  })
}
</script>

<style scoped></style>
