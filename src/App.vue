<template>
  <div id="app">
    <ImageUploader />
    <ImageGallery />
  </div>
</template>

<script>
import ImageUploader from "./components/ImageUploader.vue"; 
import ImageGallery from "./components/ImageGallery.vue"; 
export default {
  name: 'App',
  components: {
    ImageUploader, 
    ImageGallery, 
  }
}
</script>