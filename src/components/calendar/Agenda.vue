<template>
  <div class="agenda" :style="{ backgroundColor: eventColor }">
    <div class="agenda-title">
      <label>{{ eventTitle }}</label>
    </div>
  </div>
</template>

<script>
export default {
  name: "Agenda",
  props: {
    eventTitle: String,
    eventColor: String,
  },
};
</script>

<style scoped></style>
