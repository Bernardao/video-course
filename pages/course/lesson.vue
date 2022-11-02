<template>
  <article class="lesson">
    <h2>Lecciones</h2>
    <p>Esto es una lección</p>
    <p>Chapter slug: {{ $route.params.chapterSlug }}</p>
    <p>Lesson slug: {{ $route.params.lessonSlug }}</p>
  </article>
</template>

<script setup lang="ts"></script>

<style scoped></style>
