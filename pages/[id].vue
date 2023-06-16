<template>
  <v-container style="max-width: 1200px">
    <v-breadcrumbs
      bg-color="grey-darken-1 "
      class="mb-10"
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

            <p class="pt-10 text-body-1">
              <v-divider class="pt-6"></v-divider>
              Lorem ipsum dolor sit amet, consectetur adipisicing elit. Qui
              sequi similique dicta optio voluptatibus voluptas tenetur quae vel
              asperiores adipisci neque atque, expedita non repellendus itaque!
              Quasi velit expedita quidem.
            </p>
            <v-text-field
              class="py-12"
              :loading="loading"
              density="compact"
              variant="solo"
              label="Search Reviews"
              append-inner-icon="mdi-magnify"
              single-line
              hide-details
              @click:append-inner="onClick"
            ></v-text-field>
          </v-card-text>
        </v-col>
      </v-row>
    </v-card>

    <!-- <div class="d-flex flex-column mx-auto py-8">
      <div class="d-flex justify-center mt-auto text-h5">Rating overview</div>

      <div class="d-flex align-center flex-column my-auto">
        <div class="text-h2 mt-5">
          {{ product.bottomline.averageScore.toFixed(1) }}
          <span class="text-h6 ml-n3">/5</span>
        </div>

        <v-rating
          :model-value="product.bottomline.averageScore.toFixed(1)"
          color="yellow-darken-3"
          half-increments
        ></v-rating>
        <div class="px-3">{{ product.bottomline.totalReview }} ratings</div>
        <div>{{ product.bottomline.starDistribution }}</div>
      </div>

      <v-list
        bg-color="transparent"
        class="d-flex flex-column-reverse"
        density="compact"
      >
        <v-list-item v-for="(rating, i) in 5" :key="i">
          <v-progress-linear
            :model-value="rating * 15"
            class="mx-n5"
            color="yellow-darken-3"
            height="20"
            rounded
          ></v-progress-linear>

          <template v-slot:prepend>
            <span>{{ rating }}</span>
            <v-icon icon="mdi-star" class="mx-3"></v-icon>
          </template>

          <template v-slot:append>
            <div class="rating-values">
              <span class="d-flex justify-end"> {{ rating * 2 }} </span>
            </div>
          </template>
        </v-list-item>
      </v-list>
    </div> -->
  </v-container>
</template>

<script setup>
const { id } = useRoute().params;
const uri =
  "https://api-cdn.yotpo.com/v3/storefront/store/G41dFUVNuvTPYwXHBERJySdoR14ovppB8h5Swllg/product/" +
  id +
  "/reviews?page=1";

const menuItems = [
  { text: "Home", to: "/" },
  { text: "Reviews", to: "/" },
];

//  fetch the products
const { data: product } = await useFetch(uri); //, { key: id });
if (!product.value) {
  throw createError({ statusCode: 404, statusMessage: "Product not found" });
}
</script>

<style scoped></style>
