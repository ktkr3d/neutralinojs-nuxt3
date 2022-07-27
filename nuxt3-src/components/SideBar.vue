<template>
  <v-navigation-drawer v-model="drawer" :rail="rail" theme="dark" permanent expand-on-hover @click="rail = false">
    <v-list density="compact" nav>
      <v-list-item v-for="item in menuItems" :prepend-icon="item.icon" :title="item.title" :value="item.value" :to="item.to"></v-list-item>
    </v-list>
    <template v-slot:append>
      <v-btn class="mx-auto" variant="text" icon="mdi-arrow-expand-horizontal" @click.stop="rail = !rail"></v-btn>
    </template>
  </v-navigation-drawer>
</template>

<script setup lang="ts">
const drawer = useState("drawer", () => true);
const rail = useState("rail", () => true);
const menuItems = [
  { title: "Home", value: "home", to: "/", icon: "mdi-home" },
  { title: "My Account", value: "account", to: "/account", icon: "mdi-account" },
  { title: "Search", value: "search", to: "/search", icon: "mdi-magnify" },
  { title: "Cloud", value: "cloud", to: "/cloud", icon: "mdi-cloud" },
  { title: "Extensions", value: "extensions", to: "/extensions", icon: "mdi-puzzle" },
  { title: "Timeline", value: "timeline", to: "/timeline", icon: "mdi-timeline" },
  { title: "Settings", value: "settings", to: "/settings", icon: "mdi-cog" },
  { title: "3D", value: "3d", to: "/three", icon: "mdi-rotate-3d" },
  { title: 'MMD', value: 'MMD', href: '/mmd', icon: 'mdi-human-female-dance' },
];
</script>
