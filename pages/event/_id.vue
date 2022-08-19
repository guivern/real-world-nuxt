<template>
  <div>
    <h1>Event {{ this.event.title }}</h1>
    <ul style="list-style: none">
      <li>📝 <b>Description:</b> {{ this.event.description }}</li>
      <li>📍 <b>Location:</b> {{ this.event.location }}</li>
      <li>🗣 <b>Organizer:</b> {{ this.event.organizer }}</li>
      <li>📅 <b>Date:</b> {{ this.event.date.split('T')[0] }}</li>
      <li>⏰ <b>Time:</b> {{ this.event.time }} hs.</li>
    </ul>
  </div>
</template>

<script>
import { useEventStore } from '../../store/EventStore'
export default {
  async asyncData({ $axios, error, params, $pinia }) {
    try {
      const eventStore = useEventStore($pinia)
      await eventStore.fetchEvent(params.id)
      return { event: eventStore.event }
    } catch (e) {
      error({
        statusCode: 503,
        message: 'Unable to fecth event #' + params.id,
      })
    }
  },
  head() {
    return {
      title: 'Event ' + this.event.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'What you need to know about ' + this.event.title,
        },
      ],
    }
  },
}
</script>

<style></style>
