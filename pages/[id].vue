<template>
  <HeroSubPage />
  <v-container style="max-width: 1200px">
    <v-breadcrumbs
      bg-color="grey-darken-1 "
      class="mb-10 mt-10"
      :items="menuItems"
    ></v-breadcrumbs>
    <v-card elevation="6" class="pa-10">
      <v-row>
        <v-col cols="12" md="4" lg="4">
          <img
            :src="product.products[0].imageUrl"
            alt="Product Image"
            cover
            width="300"
          />
        </v-col>
        <v-col cols="12" md="8" lg="8">
          <v-card-item>
            <v-card-title class="text-h5 py-3 font-weight-bold">{{
              product.products[0].name
            }}</v-card-title>
          </v-card-item>

          <v-card-text>
            <v-row align="center" class="mx-0">
              <v-rating
                :model-value="product.bottomline.averageScore"
                color="amber"
                density="compact"
                half-increments
                readonly
                size="small"
              ></v-rating>

              <div class="text-grey ms-4">
                {{ product.bottomline.averageScore.toFixed(1) }} ({{
                  product.bottomline.totalReview
                }})
              </div>
            </v-row>

            <v-divider class="mt-8"></v-divider>
            <v-text-field
              class="py-12"
              v-model="query"
              :loading="loading"
              density="compact"
              variant="solo"
              label="Search in reviews"
              prepend-inner-icon="mdi-magnify"
              single-line
              hide-details
              @click:prepend-inner="postData"
              clearable
            ></v-text-field>

            <div v-if="response">
              <h2>Summarize</h2>
              <p class="pt-2 text-body-2 text-capitalize">
                {{ response.value.query }}
              </p>
              <p class="pt-5 text-body-1">
                {{ response.value.summarize_output }}
              </p>
              <v-divider class="mt-6"></v-divider>
              <v-list>
                <v-list-item
                  v-for="comment in response.value.search_results"
                  :key="comment.id"
                >
                  <v-list-item-content>
                    <v-list-item-title
                      class="font-weight-bold pt-5 text-capitalize"
                      >{{ comment.payload.title }}</v-list-item-title
                    >

                    <v-list-item-subtitle class="pt-2">{{
                      comment.payload.createdAt
                    }}</v-list-item-subtitle>
                    <v-list-item-subtitle class="pt-2">
                      <v-rating
                        :model-value="comment.payload.score"
                        color="success"
                        density="compact"
                        half-increments
                        readonly
                      ></v-rating>
                    </v-list-item-subtitle>
                    <v-list-item-subtitle class="pt-2">
                      <v-icon icon="mdi-chart-bar  mr-1"></v-icon
                      >{{ comment.score.toFixed(2) }}
                      <v-icon icon="mdi-emoticon-outline ml-3"></v-icon>

                      {{ comment.payload.sentiment.toFixed(2) }}
                    </v-list-item-subtitle>
                    <v-list-item-subtitle class="pt-4 text-body-1">{{
                      comment.payload.content
                    }}</v-list-item-subtitle>

                    <v-divider class="mt-7"></v-divider>

                    <!-- <v-progress-circular
                      :rotate="360"
                      :size="60"
                      :width="10"
                      :model-value="comment.score * 100"
                      color="teal"
                    >
                      {{ comment.score.toFixed(2) }}
                    </v-progress-circular>
                    <v-progress-circular
                      :rotate="360"
                      :size="60"
                      :width="10"
                      :model-value="comment.payload.sentiment * 100"
                      color="teal"
                    >
                      {{ comment.payload.sentiment.toFixed(2) }}
                    </v-progress-circular> -->
                  </v-list-item-content>
                </v-list-item>
              </v-list>
            </div>
          </v-card-text>
        </v-col>
      </v-row>
    </v-card>
  </v-container>
</template>

<script setup>
import { ref } from "vue";
const { id } = useRoute().params;

const reviewUri = "https://clm-vsearch.azurewebsites.net/api/search-reviews";

const uri =
  "https://api-cdn.yotpo.com/v3/storefront/store/G41dFUVNuvTPYwXHBERJySdoR14ovppB8h5Swllg/product/" +
  id +
  "/reviews?page=1";

const menuItems = [
  { text: "Home", to: "/" },
  { text: "Reviews", to: "/" },
];

const response = ref(null);
const loaded = ref(false);
const loading = ref(false);
const query = ref(null);

const comments = ref([
  {
    id: 1,
    author: "John",
    date: "2023-06-30",
    text: "Great product!",
    rating: 4,
  },
  { id: 2, author: "Mary", date: "2023-06-29", text: "I love it!", rating: 5 },
  {
    id: 3,
    author: "David",
    date: "2023-06-28",
    text: "Highly recommended!",
    rating: 4.5,
  },
]);

//  fetch the products
const { data: product } = await useFetch(uri); //, { key: id });
if (!product.value) {
  throw createError({ statusCode: 404, statusMessage: "Product not found" });
}

console.log(product.value);

const postData = async () => {
  try {
    loading.value = true;
    const { data: responseValue } = await useFetch(reviewUri, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: {
        query: query.value,
        product_name: product.value.products[0].name, // "GHOST WHITE",
        request_id: id, //"7f420a8c9e464483b7507b826fb96e9d",
      },
    });

    setTimeout(() => {
      loading.value = false;
      loaded.value = true;
    }, 1500);

    // Update the response value
    response.value = responseValue;

    console.log(response.value);
  } catch (error) {
    console.error(error);
  }
};
</script>

<style scoped></style>
