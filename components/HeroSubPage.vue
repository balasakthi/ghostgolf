<template>
  <v-parallax src="images/header.webp" height="300">
    <div
      class="d-flex flex-column fill-height justify-center align-center text-white"
    >
      <h1 class="text-h4 font-weight-thin mb-4">Ghost Golf Club</h1>
      <!-- <h4 class="subheading">Reviews!</h4> -->
    </div>
  </v-parallax>
</template>

<script setup>
import { onMounted, ref } from "vue";

const title = ref(null);
const subtitle = ref(null);
const { heroData } = defineProps(["heroData"]);
</script>

<style scoped></style>
