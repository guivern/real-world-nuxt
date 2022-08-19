<template>
  <div>
    <h1>Events</h1>
    <EventCard
      v-for="(event, index) in events"
      :key="index"
      :event="event"
      :data-index="index"
    />
  </div>
</template>

<script>
import EventCard from '../components/EventCard.vue'
import { useEventStore } from '../store/EventStore'
export default {
  name: 'IndexPage',
  components: { EventCard },
  setup() {
    const eventStore = useEventStore()
    return { eventStore }
  },
  async asyncData({ $pinia, error }) {
    try {
      const eventStore = useEventStore($pinia)
      await eventStore.fetchEvents()
      return { events: eventStore.events }
    } catch (e) {
      error({
        statusCode: 503,
        message: 'Unable to fecth events at this time. Please try again.',
      })
    }
  },
  data() {
    return { events: [] }
  },
  head() {
    return {
      title: 'Event Listing',
    }
  },
  watch: {
    events(newValue, oldValue) {
      window.console.log(newValue, oldValue)
    },
  },
}
</script>
