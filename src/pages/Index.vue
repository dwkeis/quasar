<template>
  <q-page class="flex flex-center">
    <div style="width:100%">
      <div class="row justify-center items-center">
        <q-btn flat label="Prev" @click="calendarPrev" />
        <q-separator vertical />
        <q-btn flat label="Next" @click="calendarNext" />
      </div>
      <q-separator />
      <q-calendar
        ref="calendar"
        v-model="selectedDate"
        view="week"
        locale="en-us"
        animated
        transition-prev="slide-right"
        transition-next="slide-left"
        @click:time2="onClickTime2"
        :interval-height="50"
        style="height: 100%;"
      />
    </div>
    <q-card
      class="events col-4 q-pa-xs full-height column justify-start items-start"
    >
      <q-item-section class="full-width">
        <q-item-label>Events</q-item-label>
        <q-item-label class="my-text-caption"
          >New data appended to top</q-item-label
        >
      </q-item-section>
      <q-separator />
      <div class="scroll overflow-auto" style="height: 360px; width: 100%;">
        <div
          v-for="(event, index) in events"
          :key="index"
          class="col-12"
          style="font-size: 10px; line-height: 10px; max-height: 14px; min-height: 14px; padding: 2px 2px; white-space: nowrap;"
        >
          {{ event }}
        </div>
      </div>
    </q-card>
  </q-page>
</template>

<script>
export default {
  name: "Dayview",
  data() {
    return {
      selectedDate: "",
      events: []
    };
  },
  methods: {
    calendarNext() {
      this.$refs.calendar.next();
    },
    calendarPrev() {
      this.$refs.calendar.prev();
    },
    onClickTime2(data) {
      this.events.unshift(`click:time2: ${JSON.stringify(data)}`);
    }
  }
};
</script>
