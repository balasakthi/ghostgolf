<template>
  <Carousels />
  <v-divider class="mt-16 mb-5"></v-divider>
  <v-container style="max-width: 1200px">
    <h2 class="font-weight-bold py-12">Products</h2>
    <v-row>
      <v-col
        cols="12"
        sm="6"
        md="4"
        lg="3"
        v-for="product in products"
        :key="product.id"
      >
        <v-card elevation="5">
          <v-img :src="product.image" cover>
            <!-- You can add additional content or overlay on the image if needed -->
          </v-img>
          <v-card-title
            class="text-left font-weight-bold text-uppercase pa-5"
            >{{ product.title }}</v-card-title
          >
          <v-divider></v-divider>
          <v-card-actions>
            <v-btn
              class="text-capitalize text-body-1"
              color="grey-darken-4 ma-2"
              variant="text"
              :to="`/${product.id}`"
              >View Details</v-btn
            >
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref } from "vue";

const products = ref([
  {
    id: 6888921792672,
    image: "/images/gary.webp",
    title: "Gary",
  },
  {
    id: 5953593933984,
    image: "/images/player.webp",
    title: "Player",
  },
  {
    id: 7006158291104,
    image: "/images/white-glove.webp",
    title: "Glove",
  },
  {
    id: 7526307725472,
    image: "/images/katana.webp",
    title: "Katana",
  },
]);
</script>

<style scoped></style>
