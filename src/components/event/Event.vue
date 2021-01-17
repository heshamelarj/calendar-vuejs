<template>
  <div class="event">

  <Toggler :show-event-form="showEventForm" :toggle-event-form="toggleEventForm"/>
  <div class="event-modal--wrapper" v-if="showEventForm">
    <Form  :show-event-form="showEventForm"
          :toggle-event-form="toggleEventForm"
    @eventCreated="createNewEvent"
    />
  </div>
  </div>

</template>


<script>
import Form from "@/components/event/Form";
import Toggler from "@/components/event/Toggler";

export default {
  components: {
    Form,
    Toggler
  },
  data: () => {

    return {

      showEventForm: false,
      events:[]


    }
  },
  props: {
    currentDayOfMonth: Number
  },
  methods: {
    toggleEventForm() {
      this.showEventForm = !this.showEventForm
    },
  createNewEvent(event){
      this.showEventForm = false
    this.events.push(event)
  }
  },
  updated() {
    this.$emit('eventsAdded',this.events)
  }


}
</script>

<style lang="scss">
@import "event.scss";
</style>