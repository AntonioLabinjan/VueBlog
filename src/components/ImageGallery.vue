<template>
  <div>
    <div v-for="(imageInfo, index) in images" :key="index">
      <img :src="imageInfo.url" alt="Slika">
      <p>Datum i vrijeme postavljanja: {{ imageInfo.date }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      images: []
    };
  }
};
</script>
