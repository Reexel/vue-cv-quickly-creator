<template>
  <p>
    {{ text }}
  </p>
</template>

<script>
export default {
  props: {
    text: String
  },
  methods: {
    showText () {
      return this.dataCV
    }
  },
  computed: {
    dataCV: function (data) {
      if (data.data.length > 0) {
        return this.data.filter(element => (element.type === 'subtitle' || element.type === 'text'))
      }
      return null
    }
  }
}
</script>

<style scoped>

</style>
