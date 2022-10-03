<template>
  <main class="font-Tech">
    <div class="h-screen w-full relative">
      <img :src="image" class="h-full w-full object-cover" />
      <div class="fixed inset-0 bg-black/40"></div>
      <ClockComponent />
    </div>
  </main>
</template>

<script setup>
import { createApi } from "unsplash-js";
import { ref } from "@vue/reactivity";
import { onMounted } from "@vue/runtime-core";
import ClockComponent from "./components/ClockComponent.vue";

const image = ref(null);

onMounted(async () => {
  await getImage();
  loopImages();
});

const getImage = async () => {
  //prepare Unsplash API connection
  const browserApi = createApi({
    accessKey: import.meta.env.VITE_UNSPLASH_KEY,
  });

  //calling Unsplash API To get the image
  var newImage = await browserApi.photos.getRandom({
    count: 1,
  });

  //display image
  image.value = newImage.response[0].links.download;
};

const loopImages = async () => {
  setInterval(getImage, 90000);
};
</script>
