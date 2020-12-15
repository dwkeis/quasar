<template>
  <q-page class="column" style="overflow: hidden">
    <!-- Even card -->

    <q-dialog v-model="addEvent" no-backdrop-dismiss>
      <div>
        <q-form ref="event" @submit="onSubmit" @reset="onReset">
          <q-card v-if="addEvent" style="width : 400px;">
            <q-toolbar class="bg-primary text-white">
              <q-toolbar-title> {{ addOrUpdateEvent }} Event </q-toolbar-title>
              <q-btn
                flat
                round
                color="white"
                icon="close"
                v-close-popup
              ></q-btn>
            </q-toolbar>
            <q-card-section>
              <q-input
                v-model="eventForm.title"
                placeholder="Title"
                :rules="[v => (v && v.length > 0) || 'Field cannot be empty']"
                autofocus
              >
                <template v-slot:before>
                  <q-icon name="title" />
                </template>
              </q-input>
              <q-input v-model="eventForm.details" placeholder="Details">
                <template v-slot:before>
                  <q-icon name="subject" />
                </template>
              </q-input>
            </q-card-section>

            <div class="q-pa-md">
              <!--Start Time-->
              <q-input
                v-model="eventForm.dateTimeStart"
                ref="dateTimeStart"
                label="Select Start Time"
                mask="####-##-## ##:##"
              >
                <template v-slot:before>
                  <q-icon name="add_task" />
                </template>
                <template v-slot:append>
                  <q-btn flat round icon="event" class="cursor-pointer">
                    <q-popup-proxy
                      transition-show="scale"
                      transition-hide="scale"
                    >
                      <q-date
                        v-model="eventForm.dateTimeStart"
                        mask="YYYY-MM-DD HH:mm"
                      >
                        <div class="row items-center justify-end">
                          <q-btn
                            v-close-popup
                            label="Close"
                            color="primary"
                            flat
                          />
                        </div>
                      </q-date>
                    </q-popup-proxy>
                  </q-btn>
                  <q-btn flat round icon="access_time" class="cursor-pointer">
                    <q-popup-proxy
                      transition-show="scale"
                      transition-hide="scale"
                    >
                      <q-time
                        v-model="eventForm.dateTimeStart"
                        mask="YYYY-MM-DD HH:mm"
                        format24h
                      >
                        <div class="row items-center justify-end">
                          <q-btn
                            v-close-popup
                            label="Close"
                            color="primary"
                            flat
                          />
                        </div>
                      </q-time>
                    </q-popup-proxy>
                  </q-btn>
                </template>
              </q-input>
              <!--End Time-->
              <q-input
                v-model="eventForm.dateTimeEnd"
                ref="dateTimeEnd"
                label="Select End Time"
                mask="####-##-## ##:##"
              >
                <template v-slot:before>
                  <q-icon name="" />
                </template>

                <template v-slot:append>
                  <q-btn flat round icon="event" class="cursor-pointer">
                    <q-popup-proxy
                      transition-show="scale"
                      transition-hide="scale"
                    >
                      <q-date
                        v-model="eventForm.dateTimeEnd"
                        mask="YYYY-MM-DD HH:mm"
                      >
                        <div class="row items-center justify-end">
                          <q-btn
                            v-close-popup
                            label="Close"
                            color="primary"
                            flat
                          />
                        </div>
                      </q-date>
                    </q-popup-proxy>
                  </q-btn>
                  <q-btn flat round icon="access_time" class="cursor-pointer">
                    <q-popup-proxy
                      transition-show="scale"
                      transition-hide="scale"
                    >
                      <q-time
                        v-model="eventForm.dateTimeEnd"
                        mask="YYYY-MM-DD HH:mm"
                        format24h
                      >
                        <div class="row items-center justify-end">
                          <q-btn
                            v-close-popup
                            label="Close"
                            color="primary"
                            flat
                          />
                        </div>
                      </q-time>
                    </q-popup-proxy>
                  </q-btn>
                </template>
              </q-input>
              <!--Check All Day box-->
              <q-field v-model="eventForm.allDay" style="padding-bottom: 20px;">
                <template v-slot:before>
                  <q-icon name="" />
                </template>
                <q-checkbox v-model="eventForm.allDay" label="All-Day event?" />
              </q-field>
            </div>

            <q-card-actions align="right">
              <q-btn flat type="reset" label="Cancel" v-close-popup></q-btn>
              <q-btn color="primary" type="submit" label="OK"></q-btn>
            </q-card-actions>
          </q-card>
        </q-form>
      </div>
    </q-dialog>

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
      @click:time2="addEventMenu"
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

const formDefault = {
  title: "",
  details: "",
  allDay: false,
  dateTimeStart: "",
  dateTimeEnd: "",
  bgcolor: "#0000FF"
};

export default {
  name: "Dayview",
  data() {
    return {
      selectedDate: "",
      datestart: "2020-12-14 01:42",
      dateend: "",
      allDay: false,
      title: "",
      details: "",
      contextDay: null,
      addEvent: false,
      eventForm: { ...formDefault },
      events: [
        {
          title: "Meeting",
          details: "Time to pitch my idea to the company",
          date: "2020-12-15",
          time: "10:00",
          duration: 120,
          bgcolor: "red",
          icon: "fas fa-handshake"
        },
        {
          title: "1st of Month",
          details:
            "Everything is funny as long as it is happening to someone else",
          date: "2020-12-16",
          time: "11:30",
          duration: 60
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
    },
    addOrUpdateEvent() {
      if (this.contextDay && this.contextDay.bgcolor) {
        return "Update";
      }
      return "Add";
    }
  },
  methods: {
    resetForm() {
      this.$set(this, "eventForm", { ...formDefault });
    },
    addEventMenu({ scope, event }) {
      this.resetForm();
      this.contextDay = { ...scope.timestamp };
      this.addEvent = true; // show dialog
    },
    onReset() {},
    onSubmit() {
      this.saveEvent();
    },
    saveEvent() {
      const self = this;
      this.$refs.event.validate().then(success => {
        if (success) {
          // close the dialog
          self.addEvent = false;
          const form = { ...self.eventForm };
          let update = false;
          if (self.contextDay.bgcolor) {
            // an update
            update = true;
          } else {
            // an add
          }
          const data = {
            title: form.title,
            details: form.details,
            bgcolor: form.bgcolor,
            date: String(form.dateTimeStart)
              .slice(0, 10)
              .replace(/\//g, "-")
          };
          if (form.allDay === false) {
            // get time into separate var
            data.time = String(form.dateTimeStart).slice(11, 16);
            data.duration = self.getDuration(
              form.dateTimeStart,
              form.dateTimeEnd,
              "minutes"
            );
          }
          if (update === true) {
            const index = self.findEventIndex(self.contextDay);
            if (index >= 0) {
              self.events.splice(index, 1, { ...data });
            }
          } else {
            // add to events array
            self.events.push(data);
          }

          self.contextDay = null;
        }
      });
    },
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
      s["background-color"] = event.bgcolor;
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
      const currentDate = QCalendar.parseTimestamp(dt);
      const events = [];
      for (let i = 0; i < this.events.length; ++i) {
        let added = false;
        const event = this.events[i];
        if (event.date === dt) {
          if (event.time !== undefined) {
            if (events.length > 0) {
              // check for overlapping times
              const startTime = QCalendar.parseTimestamp(
                event.date + " " + event.time
              );
              const endTime = QCalendar.addToDate(startTime, {
                minute: event.duration
              });
              for (let j = 0; j < events.length; ++j) {
                const evt = events[j];
                if (evt.time !== undefined) {
                  const startTime2 = QCalendar.parseTimestamp(
                    evt.date + " " + evt.time
                  );
                  const endTime2 = QCalendar.addToDate(startTime2, {
                    minute: evt.duration
                  });
                  if (
                    QCalendar.isBetweenDates(startTime, startTime2, endTime2) ||
                    QCalendar.isBetweenDates(endTime, startTime2, endTime2)
                  ) {
                    evt.side = "left";
                    event.side = "right";
                    events.push(event);
                    added = true;
                    break;
                  }
                }
              }
            }
          }
          if (!added) {
            event.side = undefined;
            events.push(event);
          }
        } else if (event.days) {
          // check for overlapping dates
          const startDate = QCalendar.parseTimestamp(event.date);
          const endDate = QCalendar.addToDate(startDate, { day: event.days });
          if (QCalendar.isBetweenDates(currentDate, startDate, endDate)) {
            events.push(event);
            added = true;
          }
        }
      }
      return events;
    }
  }
};
</script>
