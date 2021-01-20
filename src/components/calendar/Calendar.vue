<template>
  <div>
    <div class="month-switcher">
      <button class="month-switcher-left--button" @click="showPrevMonth">
        <span class="prev-month--label">PREV</span>
      </button>
      <div class="month-title">
        <label class="month-title--label">{{ getMonthName }}</label>
      </div>
      <button class="month-switcher-right--button">
        <span class="next-month--label">NEXT</span>
      </button>
    </div>
    <ShowMonth
      :current-day-of-month="currentDayOfMonth"
      :events="events"
      :highlight-class="highlightClass"
      :weekdays-names="weekdaysNames"
      :weeks="weeks"
      :get-day-and-month-from-event-date="getDayAndMonthFromEventDate"
    />
    <Event :currentDayOfMonth="currentDayOfMonth" @eventsAdded="setEvents" />
  </div>
</template>

<script>
import ShowMonth from "@/components/calendar/ShowMonth";
import Event from "../event/Event";
export default {
  components: { ShowMonth, Event },
  data: () => {
    const date = new Date();
    return {
      date,
      currentYear: date.getUTCFullYear(),
      updatedYear: date.getUTCFullYear(),
      currentDayOfMonth: date.getUTCDate(),
      currentDayOfWeek: date.getDay(),
      currentMonth: date.getUTCMonth(),
      updatedMonth: date.getUTCMonth(),
      monthName: "",
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
    getMonthName() {
      const monthNames = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];

      return monthNames[this.currentMonth];
    },
    getPrevMonth() {
      let currentMonth = this.currentMonth;

      if (currentMonth === 0) {
        this.updateYear(this.currentYear - 1);
        this.updateMonth(11);
        currentMonth = 11;
      } else {
        currentMonth--;
      }
      this.updateMonth(currentMonth);
      return currentMonth;
    },
    getNextMonth() {
      let currentMonth = this.currentMonth;
      let updatedMonth = this.updatedMonth;
      if (currentMonth === 11) {
        this.updateYear(this.currentYear + 1);
        this.updateMonth(0);
      } else {
        updatedMonth++;
      }
      return updatedMonth;
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

    updateYear(year) {
      this.currentYear = year;
    },
    updateMonth(month) {
      this.currentMonth = month;
      console.log(month);
    },
    showPrevMonth() {
      this.currentMonth = this.getPrevMonth;
      console.log(this.currentMonth);
      console.log(this.getCurrentMonthDays);
    },
  },
  mounted() {
    this.initCalendar();
  },
  updated() {
    console.log("updated");
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
@import "calendar";
</style>
