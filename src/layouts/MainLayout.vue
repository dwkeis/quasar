<template>
  <div class="q-pa-md">
    <q-layout view="hHh Lpr lff">
      <q-header elevated>
        <q-toolbar>
          <q-btn
            flat
            @click="drawerLeft = !drawerLeft"
            round
            dense
            icon="menu"
          />
          <q-toolbar-title>
            KC's first test
          </q-toolbar-title>

          <div>Quasar v{{ $q.version }}</div>
        </q-toolbar>
      </q-header>

      <q-drawer
        v-model="drawerLeft"
        show-if-above
        :breakpoint="700"
        :width="310"
        elevated
      >
        <q-scroll-area class="fit">
          <div class="q-pa-sm">
            <q-date v-model="date" />
          </div>
        </q-scroll-area>
      </q-drawer>

      <q-page-container>
        <router-view />
      </q-page-container>
    </q-layout>
  </div>
</template>

<script>
export default {
  name: "MainLayout",
  data() {
    return {
      drawerLeft: false,
      date: "2020/12/07"
    };
  }
};
</script>
