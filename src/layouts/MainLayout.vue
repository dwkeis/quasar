<template>
  <div class="q-pa-md">
    <q-layout view="hHh Lpr lff" >
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
              <q-tooltip content-class="bg-grey">{{ today }}</q-tooltip>
            </q-btn>
            <q-btn icon="keyboard_arrow_left" round flat size="10px">
              <q-tooltip content-class="bg-grey">上週</q-tooltip>
            </q-btn>
            <q-btn
              icon="keyboard_arrow_right"
              round
              flat
              size="10px"
              class="q-mr-md q-my-md"
            >
            <q-tooltip content-class="bg-grey">下週</q-tooltip>
            </q-btn>
            <q-btn flat>
              {{ noday }} <br> 農曆N月
              <q-popup-proxy @before-show="updateProxy" transition-show="scale" transition-hide="scale">
                <q-date v-model="proxyDate" minimal>
                  <div class="row items-center justify-end q-gutter-sm" />
                </q-date>
              </q-popup-proxy>
            </q-btn>
          </q-toolbar-title>

          <q-btn icon="search" round flat class="q-mr-md q-my-md"/>
          <q-btn icon="contact_support" round flat class="q-mr-md q-my-md"/>
          <q-btn icon="settings" round flat class="q-mr-md q-my-md"/>
          <q-btn-dropdown label="週" round padding="5px 8px">
            <q-list>
              <q-item clickable v-close-popup @click="onItemClick">
                <q-item-section>
                  <q-item-label>週</q-item-label>
                </q-item-section>
              </q-item>

              <q-item clickable v-close-popup @click="onItemClick">
                <q-item-section>
                  <q-item-label>月</q-item-label>
                </q-item-section>
              </q-item>

              <q-item clickable v-close-popup @click="onItemClick">
                <q-item-section>
                  <q-item-label>年</q-item-label>
                </q-item-section>
              </q-item>
            </q-list>
          </q-btn-dropdown>
          <q-btn icon="apps" round flat class="q-ml-md"/>
          <q-btn icon="account_circle" round flat />
        </q-toolbar>
      </q-header>

      <q-drawer
        v-model="drawerLeft"
        show-if-above
        :breakpoint="700"
        :width="310"
      >
      
        <q-scroll-area class="fit">
          <br>
          <q-btn
            size="30px"
            round
            icon="add"
          />
          <br>
          <br>
          <div class="q-pa-sm">
            <q-date v-model="date" today-btn/>
          </div>
          <q-btn-dropdown label="我的日曆" class="full-width" flat>
            <q-list>
              <q-item clickable v-close-popup @click="onItemClick">
                <q-item-section>
                  <q-item-label>Kc Ma</q-item-label>
                </q-item-section>
              </q-item>

              <q-item clickable v-close-popup @click="onItemClick">
                <q-item-section>
                  <q-item-label>提醒</q-item-label>
                </q-item-section>
              </q-item>

              <q-item clickable v-close-popup @click="onItemClick">
                <q-item-section>
                  <q-item-label>Contacts</q-item-label>
                </q-item-section>
              </q-item>
            </q-list>
          </q-btn-dropdown>


          <q-btn label="其他日曆" class="full-width" flat>
            <q-menu>
              <q-list style="full-width">
                <q-item clickable v-close-popup >
                  <q-item-section>
                    週
                  </q-item-section>
                </q-item>

                <q-item clickable v-close-popup>
                  <q-item-section>
                    月
                  </q-item-section>
                </q-item>

                <q-item clickable v-close-popup>
                  <q-item-section>
                    年
                  </q-item-section>
                </q-item>
              </q-list>
            </q-menu>
          </q-btn>

        </q-scroll-area>
      </q-drawer>

      <q-page-container>
        <router-view />
      </q-page-container>
    </q-layout>
  </div>
</template>

<script>
var Today=new Date();
var month = Today.getMonth() + 1;
var fullday = Today.getFullYear() + "/" + month + "/" + Today.getDate();
export default {
  name: "MainLayout",
  data() {
    return {
      drawerLeft: false,
      proxyDate: fullday,
      date: fullday,
      today: Today.getFullYear() + "年" + Today.getMonth() + "月" + Today.getDate() + "日",
      noday: Today.getFullYear() + "年" + month + "月"
    };
  }
};
</script>
