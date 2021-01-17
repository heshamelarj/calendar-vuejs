<template>
  <button v-if="!showEventForm" @click="toggleEventForm">
    Add Event
  </button>
</template>

<script>
export default {
name: "Toggler",
  props: {
    showEventForm: Boolean,
    toggleEventForm: Function
  }
}
</script>

<style scoped>

</style>