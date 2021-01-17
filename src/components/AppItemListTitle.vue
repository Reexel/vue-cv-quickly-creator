<template>
  <h1>{{ dataTitle }}</h1>
</template>

<script>
export default {
  props: ['data'],
  methods: {
    showTitle () {
      return this.dataTitle
    }
  },
  computed: {
    dataTitle: function (data) {
      if (data.data.length > 0) {
        const title = data.data.filter(element => element.type === 'title')
        if (title.length > 0) {
          return title[0].value
        }
      }
      return null
    }
  }
}
</script>

<style scoped>

</style>
