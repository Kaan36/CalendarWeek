<template>
  <div id="calender-week">
    <div class="card-group">
      <!-- Anfang: Template für die Calendar-Day-Component -->
      <CalendarDay :day="day" v-for="day in calendarWeekData" :key="day.id" />
      <!-- Ende: Template für die Calendar-Day-Component -->
    </div>
  </div>
</template>

<script>
import CalendarDay from "./CalendarDay.vue";
import Store from "../store.js";

export default {
  name: "CalenderWeek",
  components: {
    CalendarDay,
  },
  data() {
    return {
      calendarWeekData: Store.state.calendarWeekData,
    };
  },
};
</script>

<style scoped>
h5 {
  color: red;
}
</style>
