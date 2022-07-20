<template>
  <v-card>
    <v-layout>
      <v-navigation-drawer
        v-model="drawer"
        :rail="rail"
        theme="dark"
        permanent
        expand-on-hover
        @click="rail = false"
      >
        <v-list density="compact" nav>
          <v-list-item
            prepend-icon="mdi-home"
            title="Home"
            value="home"
          ></v-list-item>
          <v-list-item
            prepend-icon="mdi-account"
            title="My Account"
            value="account"
          ></v-list-item>
          <v-list-item
            prepend-icon="mdi-magnify"
            title="Search"
            value="search"
          ></v-list-item>
          <v-list-item
            prepend-icon="mdi-cloud"
            title="Cloud"
            value="cloud"
          ></v-list-item>
          <v-list-item
            prepend-icon="mdi-puzzle"
            title="Extensions"
            value="extensions"
          ></v-list-item>
          <v-list-item
            prepend-icon="mdi-cog"
            title="Settings"
            value="settings"
          ></v-list-item>
        </v-list>
        <template v-slot:append>
          <v-btn
            class="mx-auto"
            variant="text"
            icon="mdi-arrow-expand-horizontal"
            @click.stop="rail = !rail"
          ></v-btn>
        </template>
      </v-navigation-drawer>
      <v-main>
        <NuxtWelcome />
      </v-main>
    </v-layout>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      drawer: true,
      items: [
        { title: "Home", icon: "mdi-home" },
        { title: "My Account", icon: "mdi-account" },
        { title: "Users", icon: "mdi-account-group-outline" },
        { title: "Settings", icon: "mdi-cog" },
      ],
      rail: true,
    };
  },
};
</script>
