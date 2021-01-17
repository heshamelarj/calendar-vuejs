<template>
  <div class="agenda" :style="{ backgroundColor: eventColor }">
    <div class="agenda-title">
      <label>{{ eventTitle }}</label>
    </div>
    <div class="agenda-description">
      <p>{{ description }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Agenda",
  props: {
    eventTitle: String,
    description: String,
    eventStartDate: Date,
    eventEndDate: Date,
    eventColor: String,
  },
};
</script>

<style scoped></style>
