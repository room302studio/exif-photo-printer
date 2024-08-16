<template>
  <div>
    <!-- make an SVG with the image in the center and a black border, with the exif data overlaid on the border in white monotype -->
    <svg>
    </svg>
  </div>
</template>

<script setup>
const { file } = defineProps({
  file: Object,
})

onMounted(() => {
  console.log("PhotoWithExif mounted", file)
})

</script>

<style scoped></style>