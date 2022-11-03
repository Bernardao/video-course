<script lang="ts" setup>
const props = defineProps({
  videoId: {
    type: Number,
    required: true,
  },
})
</script>

<template>
  <div class="video-player">
    <iframe
      width="560"
      height="315"
      :src="`https://player.vimeo.com/video/${props.videoId}`"
      title="Video Player"
      frameborder="0"
      allow="accelerometer; autoplay; clipboard-white; encrypted-media; gyroscope; picture-in-picture"
      allowfullscreen
    >
    </iframe>
  </div>
</template>

<style scoped></style>
