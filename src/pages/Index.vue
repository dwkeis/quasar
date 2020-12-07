<template>
  <q-page class="flex flex-center">
    <div style="width:100%">
      <q-calendar
        v-model="selectedDate"
        view="week"
        locale="en-us"
        :interval-height="50"
        style="height: 100%;"
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
