<template>
  <q-page class="flex flex-center">
    <div style="max-width: 800px; width: 100%;">
      <q-calendar
        v-model="selectedDate"
        view="3day"
        locale="en-us"
        style="height: 400px;"
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
