<template>
  <div class="card card-w70">
    <h3 v-if="data.length === 0">Добавьте первый блок, чтобы увидеть результат</h3>

    <div v-else>
      <component
        :is="componentName(cv.type)"
        v-for="cv in data"
        :text="cv.value"
        :key="cv.id"
      ></component>

    </div>
  </div>
</template>

<script>
import AppItemListTitle from '@/components/AppItemListTitle'
import AppItemListText from '@/components/AppItemListText'
import AppItemListAvatar from '@/components/AppItemListAvatar'
import AppItemListSubtitle from '@/components/AppItemListSubtitle'
export default {
  props: ['data'],
  components: {
    AppItemListTitle,
    AppItemListText,
    AppItemListAvatar,
    AppItemListSubtitle
  },
  methods: {
    componentName (name) {
      return 'AppItemList' + name[0].toUpperCase() + name.substring(1)
    }
  }
}
</script>

<style scoped>

</style>
