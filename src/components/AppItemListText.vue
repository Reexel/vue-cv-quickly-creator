<template>
  <div v-for="cv in dataCV" :key="cv.id">
    <h2 v-if="cv.type === 'subtitle'">{{ cv.type === 'subtitle' ? cv.value : ''}}</h2>
    <p v-if="cv.type === 'text'">
      {{ cv.type === 'text' ? cv.value : ''}}
    </p>
  </div>
</template>

<script>
export default {
  props: ['data'],
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
