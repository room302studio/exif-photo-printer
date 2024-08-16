<template>
  <div>
    <div class="p-8 lg:p-24">
      <form id="drop-form" ref="dropForm" @drop="fileDrop($event)" @dragover="fileDragover" @dragleave="fileDragleave"
        :class="[
          fileOver
            ? 'bg-light-gray border border-green-500 border-2'
            : 'bg-light-gray',
          'w-full h-screen-25 py-4 flex flex-col justify-center items-center',
        ]">
        <span class="drop-files">📂 Drop your photos here</span>
      </form>
    </div>

    <div v-for="file in files" :key="file.name">
      <PhotoWithExif :file="file" />
    </div>

    <!-- {{ filesArray }} -->
    <UTable :rows="filesArray" />
  </div>
</template>
<script setup>
const { $toast } = useNuxtApp();
import exifr from 'exifr'

const fileRef = ref(null);
const fileOver = ref(false);

const files = ref(null)
const filesArray = ref([])


function fileDragover(event) {
  event.preventDefault();
  fileOver.value = true;
}

function fileDragleave(event) {
  event.preventDefault();
  fileOver.value = false;
}


async function fileDrop(event) {
  event.preventDefault();
  console.log("File dropped", event);

  // we only accept 1 file as input for now
  // fileRef.value = event.dataTransfer.files[0];

  // add the files to the ref
  files.value = event.dataTransfer.files;

  // this is a FileList, which is not an array
  // so we need to convert it to an array
  // we will use a for-of loop to do this
  for (const file of files.value) {
    // add the file to the array
    // lets get the file name and EXIF data for each file
    const exif = await exifr.parse(file)

    // pull out all the exif data we want
    const { Make, Model, ExposureTime, FNumber, FocalLength, DateTimeOriginal } = exif

    const fileName = file.name;

    const fileObj = {
      name: fileName,
      Make,
      Model,
      ExposureTime, FNumber, FocalLength, DateTimeOriginal,
      // exif
      imageUrl: URL.createObjectURL(file)
    }

    filesArray.value.push(fileObj)
  }
}
</script>

<style>
.h-screen-25 {
  height: 25vh;
}
</style>