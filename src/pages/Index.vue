<template>
  <q-page class="flex flex-center">
    <div style="width:100%">
      <q-calendar
        v-model="selectedDate"
        view="3day"
        locale="en-us"
        :interval-height="50"
        style="height: 1000px;"
      />
    </div>
  </q-page>
</template>

<script>
export default {
  name: "Dayview",
  data() {
    return {
      selectedDate: ""
    };
  }
};
</script>
