<template>
  <form
    class="card card-w30"
    @submit.prevent="createData"
  >
    <div class="form-control">
      <label for="type">Тип блока</label>
      <select id="type" v-model="type">
        <option value="title">Заголовок</option>
        <option value="subtitle">Подзаголовок</option>
        <option value="avatar">Аватар</option>
        <option value="text">Текст</option>
      </select>
    </div>

    <div class="form-control">
      <label for="value">Значение</label>
      <textarea
        id="value"
        rows="3"
        v-model="value"
      ></textarea>
    </div>

    <button class="btn primary" @click="addData" :disabled="value.length < countEnableAddButton">Добавить</button>
  </form>
</template>

<script>
export default {
  emits: ['add-data'],
  props: ['createData'],
  data () {
    return {
      type: 'title',
      value: '',
      countEnableAddButton: process.env.COUNT_ENABLE_ADD_BUTTON
    }
  },
  methods: {
    addData () {
      this.$emit('add-data', this.value, this.type)
      this.type = 'title'
      this.value = ''
    }
  }
}
</script>

<style scoped>

</style>
