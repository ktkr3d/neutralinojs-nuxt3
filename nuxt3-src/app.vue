<template>
  <v-card>
    <v-layout>
      <v-navigation-drawer v-model="drawer" :rail="rail" theme="dark" permanent expand-on-hover @click="rail = false">
        <v-list density="compact" nav>
          <v-list-item prepend-icon="mdi-home" title="Home" value="home" href="/"></v-list-item>
          <v-list-item prepend-icon="mdi-account" title="My Account" value="account" href="/account"></v-list-item>
          <v-list-item prepend-icon="mdi-magnify" title="Search" value="search" href="/search"></v-list-item>
          <v-list-item prepend-icon="mdi-cloud" title="Cloud" value="cloud" href="/cloud"></v-list-item>
          <v-list-item prepend-icon="mdi-puzzle" title="Extensions" value="extensions" href="/extensions"></v-list-item>
          <v-list-item prepend-icon="mdi-timeline" title="Timeline" value="timeline" href="/history"></v-list-item>
          <v-list-item prepend-icon="mdi-cog" title="Settings" value="settings" href="/settings"></v-list-item>
        </v-list>
        <template v-slot:append>
          <v-btn class="mx-auto" variant="text" icon="mdi-arrow-expand-horizontal" @click.stop="rail = !rail"></v-btn>
        </template>
      </v-navigation-drawer>
      <v-main>
        <NuxtPage />
      </v-main>
    </v-layout>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      drawer: true,
      rail: true,
    }
  },
}
</script>