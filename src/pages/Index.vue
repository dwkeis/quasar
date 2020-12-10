<template>
  <q-page class="flex flex-center">
    <div class="row justify-center items-center">
      <q-btn flat label="Prev" @click="calendarPrev" />
      <q-separator vertical />
      <q-btn flat label="Next" @click="calendarNext" />
    </div>
    <q-calendar
      ref="calendar"
      v-model="selectedDate"
      view="week"
      locale="en-us"
      style="height: 400px;"
      class="calendar-container"
      animated
      transition-prev="slide-right"
      transition-next="slide-left"
    >
      <!-- eslint-disable vue/no-unused-vars -->
      <template #day-body="{ timestamp, timeStartPos, timeDurationHeight }">
        <template v-for="(event, index) in getEvents(timestamp.date)">
          <q-badge
            v-if="event.time"
            class="absolute-center"
            :key="index"
            :style="
              badgeStyles(event, 'body', timeStartPos, timeDurationHeight)
            "
            :class="badgeClasses(event, 'body')"
          >
            <span class="ellipsis">{{ event.title }}</span>
          </q-badge>
        </template>
      </template>
    </q-calendar>

    <q-card
      class="events col-4 q-pa-xs full-height column justify-start items-start"
    >
      <q-item-section class="full-width">
        <q-item-label>Events</q-item-label>
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
import QCalendar from "@quasar/quasar-ui-qcalendar";

export default {
  name: "Dayview",
  data() {
    return {
      selectedDate: "",
      events: [
        {
          title: "Meeting",
          details: "Time to pitch my idea to the company",
          date: "2020-12-11",
          time: "10:00",
          duration: 120,
          bgcolor: "red",
          icon: "fas fa-handshake"
        },
        {
          title: "1st of Month",
          details:
            "Everything is funny as long as it is happening to someone else",
          date: "2020-12-10",
          time: "12:00",
          duration: 60,
          bgcolor: "orange"
        },
        {
          title: "Lunch",
          details: "Company is paying!",
          date: "2020-12-17",
          time: "11:30",
          duration: 90,
          bgcolor: "teal"
        }
      ]
    };
  },
  computed: {
    // convert the events into a map of lists keyed by date
    eventsMap() {
      const map = {};
      this.events.forEach(event =>
        (map[event.date] = map[event.date] || []).push(event)
      );
      return map;
    }
  },
  methods: {
    calendarNext() {
      this.$refs.calendar.next();
    },
    calendarPrev() {
      this.$refs.calendar.prev();
    },
    badgeClasses(event, type) {
      const isHeader = type === "header";
      return {
        "full-width": !isHeader && (!event.side || event.side === "full"),
        "left-side": !isHeader && event.side === "left",
        "right-side": !isHeader && event.side === "right"
      };
    },

    badgeStyles(event, type, timeStartPos, timeDurationHeight) {
      const s = {};
      if (timeStartPos) {
        s.top = timeStartPos(event.time) + "px";
      }
      if (timeDurationHeight) {
        s.height = timeDurationHeight(event.duration) + "px";
      }
      s["align-items"] = "flex-start";
      return s;
    },
    getEvents(dt) {
      const currentDate = QCalendar.parsed(dt);
      const events = [];
      for (let i = 0; i < this.events.length; ++i) {
        let added = false;
        if (this.events[i].date === dt) {
          if (this.events[i].time) {
            if (events.length > 0) {
              // check for overlapping times
              const startTime = QCalendar.parsed(
                this.events[i].date + " " + this.events[i].time
              );
              const endTime = QCalendar.addToDate(startTime, {
                minute: this.events[i].duration
              });
              for (let j = 0; j < events.length; ++j) {
                if (events[j].time) {
                  const startTime2 = QCalendar.parsed(
                    events[j].date + " " + events[j].time
                  );
                  const endTime2 = QCalendar.addToDate(startTime2, {
                    minute: events[j].duration
                  });
                  if (
                    QCalendar.isBetweenDates(startTime, startTime2, endTime2) ||
                    QCalendar.isBetweenDates(endTime, startTime2, endTime2)
                  ) {
                    events[j].side = "left";
                    this.events[i].side = "right";
                    events.push(this.events[i]);
                    added = true;
                    break;
                  }
                }
              }
            }
          }
          if (!added) {
            this.events[i].side = undefined;
            events.push(this.events[i]);
          }
        } else if (this.events[i].days) {
          // check for overlapping dates
          const startDate = QCalendar.parsed(this.events[i].date);
          const endDate = QCalendar.addToDate(startDate, {
            day: this.events[i].days
          });
          if (QCalendar.isBetweenDates(currentDate, startDate, endDate)) {
            events.push(this.events[i]);
            added = true;
          }
        }
      }
      return events;
    }
  }
};
</script>
