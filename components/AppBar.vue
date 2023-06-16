<template>
  <v-navigation-drawer v-model="drawer" temporary>
    <v-list-item
      prepend-avatar="/images/Ghost-Golf-Logo.webp"
      title="Ghost Golf"
    ></v-list-item>
    <v-divider></v-divider>
    <v-list>
      <NuxtLink
        class="text-decoration-none"
        v-for="item in menuItems"
        :key="item.title"
        :to="item.path"
      >
        <v-list-item
          class="text-grey-darken-4"
          link
          :prepend-icon="item.icon"
          :title="item.title"
          :value="item.path"
        ></v-list-item>
      </NuxtLink>
    </v-list>
  </v-navigation-drawer>

  <v-app-bar flat class="d-flex justify-center" height="100">
    <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
    <v-spacer></v-spacer>
    <v-avatar
      class="ml-6"
      size="80"
      image="/images/Ghost-Golf-Logo.webp"
    ></v-avatar>

    <v-spacer></v-spacer>
    <v-btn icon>
      <v-icon>mdi-account-outline </v-icon>
    </v-btn>
    <v-btn icon>
      <v-icon>mdi-magnify</v-icon>
    </v-btn>
    <v-btn icon>
      <v-icon>mdi-shopping-outline</v-icon>
    </v-btn>
    <v-btn icon>
      <v-icon>mdi-dots-vertical</v-icon>
    </v-btn>
  </v-app-bar>
</template>

<script>
export default {
  data: () => ({
    drawer: null,
    menuItems: [
      { id: 1, title: "Home", path: "/", icon: "mdi-home" },
      { id: 2, title: "About", path: "/about", icon: "mdi-forum" },
    ],
  }),
};
</script>

<style scoped></style>
