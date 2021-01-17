<template>
  <div class="event-modal--wrapper" v-if="showEventForm">
    <div class="event-form--modal">
      <form action="">
        <div class="title">
          <input
            type="text"
            id="title"
            :placeholder="eventTitle"
            v-model="eventTitle"
          />
        </div>
        <div class="description">
          <textarea :placeholder="description" v-model="description" />
        </div>
        <div class="event-times">
          <div class="start-date">
            <input type="date" id="startDate" v-model="eventStartDate" />
          </div>
          <div class="end-date">
            <input type="date" id="endDate" v-model="eventEndDate" />
          </div>
        </div>
        <div class="event-color">
          <select
            name="color"
            id="color"
            v-model="eventColor"
            :style="{ backgroundColor: eventColor }"
          >
            <option :value="null" :selected="true">Select a Color</option>
            <option
              v-for="(color, index, key) in colors"
              :key="key"
              :value="color"
              :style="{ backgroundColor: color }"
            >
              {{ color }}
            </option>
          </select>
        </div>
        <div class="controls">
          <button @click="toggleEventForm">X</button>
          <button @click.prevent="appendNewEvent">Create Event</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "Form",
  data: () => {
    return {
      currentDate: "",
      eventTitle: "Add title",
      description: "Add description",
      eventStartDate: "",
      eventEndDate: "",
      eventColor: "#ffffff",
      colors: ["#111d5e", "#c70039", "#f37121", "#c0e218"],
    };
  },
  props: {
    showEventForm: Boolean,
    toggleEventForm: Function,
  },
  computed: {
    getCurrentDate() {
      let today = new Date();
      const dd = String(today.getDate()).padStart(2, "0");
      const mm = String(today.getMonth() + 1).padStart(2, "0"); //January is 0!
      const yyyy = today.getFullYear();

      return yyyy + "-" + mm + "-" + dd;
    },
  },
  methods: {
    appendNewEvent() {
      let eventTitle = this.eventTitle;
      let description = this.description;
      let eventStartDate = this.eventStartDate;
      let eventEndDate = this.eventEndDate;
      let eventColor = this.eventColor;

      this.$emit("eventCreated", {
        eventTitle: eventTitle,
        description: description,
        eventStartDate: eventStartDate,
        eventEndDate: eventEndDate,
        eventColor: eventColor,
      });
    },
    initEventDates() {
      this.eventStartDate = this.getCurrentDate;
      this.eventEndDate = this.getCurrentDate;
    },
  },
  mounted() {
    this.initEventDates();
  },
};
</script>

<style scoped></style>
