<template>
  <table class="month-page--table">
    <thead>
      <tr>
        <th v-for="weekdayName in weekdaysNames" :key="weekdayName">
          {{ weekdayName }}
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="week in weeks" :key="week">
        <td
          v-for="day in week"
          v-bind:class="currentDayOfMonth === day ? highlightClass : ''"
          :key="day"
          class="day"
        >
          <span class="day-text">{{ day }}</span>
          <div v-for="(event, index, key) in events" :key="key">
            <!--FIXME add multiple days event support-->
            <div
              v-if="
                day === getDayAndMonthFromEventDate(event.eventStartDate).day ||
                day === getDayAndMonthFromEventDate(event.eventEndDate).day
              "
            >
              <Agenda
                :event-title="event.eventTitle"
                :event-color="event.eventColor"
              />
            </div>
          </div>
        </td>
      </tr>
    </tbody>
  </table>
</template>
<script>
import Agenda from "@/components/calendar/Agenda";
export default {
  name: "ShowMonth",
  props: {
    currentDayOfMonth: Number,
    weekdaysNames: Array,
    weeks: Array,
    events: Array,
    highlightClass: String,
    getDayAndMonthFromEventDate: Function,
  },
  components: {
    Agenda,
  },
};
</script>

<style scoped></style>
