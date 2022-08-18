<template>
  <h1>Event {{ this.event.title }}</h1>
</template>

<script>
export default {
  async fetch({ $axios, error, params, store }) {
    try {
      await store.dispatch('events/fetchEvent', params.id)
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
  computed: {
    id() {
      return this.$route.params.id
    },
    event() {
      return this.$store.state.events.event
    }
  },
}
</script>

<style></style>
