<template>
  <div>
    <table class="calendar">
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
          >
            {{ day }}
            <div v-for="(event, index, key) in events" :key="key">
              <div
                v-if="
                  day === getDayAndMonthFromEventDate(event.eventStartDate).day
                "
              >
                <Agenda
                  :event-title="event.eventTitle"
                  :description="event.description"
                  :event-color="event.eventColor"
                />
              </div>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    <Event
      v-bind:currentDayOfMonth="currentDayOfMonth"
      @eventsAdded="setEvents"
    />
  </div>
</template>

<script>
import Event from "../event/Event";
import Agenda from "@/components/calendar/Agenda";
export default {
  components: { Event, Agenda },
  data: () => {
    const date = new Date();
    return {
      date,
      currentYear: date.getUTCFullYear(),
      currentDayOfMonth: date.getUTCDate(),
      currentDayOfWeek: date.getDay(),
      currentMonth: date.getUTCMonth(),
      weekdaysNames: [
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
        "Sunday",
      ],
      weeks: [],
      events: [],
      highlightClass: "highlight",
    };
  },
  computed: {
    getCurrentMonthDays() {
      return new Date(this.currentYear, this.currentMonth, 0).getDate();
    },
    getFirstDayOfCurrentMonthWeekDay() {
      let weekDay = this.currentDayOfWeek;
      let month = [];
      let currentDay = this.currentDayOfMonth;
      while (currentDay >= 1) {
        if (weekDay === 0) {
          month.push(weekDay--);
          currentDay--;
          weekDay = 6;
        }
        month.push(weekDay--);
        currentDay--;
      }
      return month.reverse()[0];
    },
    getCurrentMonthsWeeks() {
      let month = [];
      let week = [];
      let currentDay = 1;
      let currentMonthDays = this.getCurrentMonthDays;
      let firstDayOfCurrentMonth = this.getFirstDayOfCurrentMonthWeekDay;
      let weekNames = this.weekdaysNames;
      let isFirstDayOfMonthSet = false;
      for (
        let weekCounter = 0;
        weekCounter < Math.ceil(currentMonthDays / 7);
        weekCounter++
      ) {
        weekNames.map((name, index) => {
          if (index + 1 === firstDayOfCurrentMonth && !isFirstDayOfMonthSet) {
            isFirstDayOfMonthSet = true;
          }
          if (!isFirstDayOfMonthSet) week.push(null);
          else week.push(currentDay++);
        });

        month.push(week);
        week = [];
      }

      return month;
    },
  },
  methods: {
    initCalendar: function () {
      this.weeks = this.getCurrentMonthsWeeks;
    },
    setEvents: function (events) {
      this.events = events;
    },
    getDayAndMonthFromEventDate(eventDate) {
      return {
        year: parseInt(eventDate.split("-")[0], 10),
        month: parseInt(eventDate.split("-")[1], 10),
        day: parseInt(eventDate.split("-")[2], 10),
      };
    },
    setDaysEvents: function (event) {
      if (event) {
        let eventStartDateObject = this.getDayAndMonthFromEventDate(
          event.eventStartDate
        );
        let eventEndDateObject = this.getDayAndMonthFromEventDate(
          event.eventEndDate
        );
        let dayEvent = {};
        this.weeks.map((week) => {
          week.find((day) => {
            if (eventStartDateObject.day === day) {
              dayEvent = {
                eventStartDay: day,
                eventEndDay: null,
                event,
              };
            }
            if (eventEndDateObject.day === day) {
              dayEvent.eventEndDay = day;
            }
          });
        });
        return dayEvent;
      } else return null;
    },
  },
  mounted() {
    this.initCalendar();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
@import "calendar";
</style>
