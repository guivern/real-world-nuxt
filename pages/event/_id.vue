<template>
  <h1>Event {{ this.event.title }}</h1>
</template>

<script>
export default {
  async asyncData({ $axios, error, params }) {
    try {
      const response = await $axios.get(
        'http://localhost:3000/events/' + params.id
      )
      return { event: response.data }
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
  },
}
</script>

<style></style>
