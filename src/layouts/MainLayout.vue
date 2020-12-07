<template>
  <div class="q-pa-md">
    <q-layout view="hHh Lpr lff">
      <q-header elevated class="bg-white text-black" height-hint="">
        <q-toolbar>
          <q-btn
            flat
            @click="drawerLeft = !drawerLeft"
            round
            dense
            icon="menu"
          />
          <q-toolbar-title>
            <q-icon name="today" />
            Calendar

            <q-btn
              color="white"
              text-color="black"
              label="今天"
              class="q-ml-xl q-mr-md"
              outline
            >
              <q-tooltip content-class="bg-grey">{{ date }}</q-tooltip>
            </q-btn>
            <q-btn icon="keyboard_arrow_left" round flat size="10px" />
            <q-btn
              icon="keyboard_arrow_right"
              round
              flat
              size="10px"
              class="q-mr-md"
            />
            <q-btn flat>
              {{ date }}
            </q-btn>
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
          <p>我的日曆</p>
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
