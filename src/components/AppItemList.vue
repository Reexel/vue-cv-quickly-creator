<template>
  <div class="card card-w70">
    <h3 v-if="!showTitle && !showAvatar && !showDataCV">Добавьте первый блок, чтобы увидеть результат</h3>

    <div v-else>
      <app-item-list-title ref="title" :data="data"></app-item-list-title>
      <app-item-list-avatar ref="avatar" :data="data"></app-item-list-avatar>
      <app-item-list-text ref="text" :data="data"></app-item-list-text>
    </div>
  </div>
</template>

<script>
import AppItemListTitle from '@/components/AppItemListTitle'
import AppItemListText from '@/components/AppItemListText'
import AppItemListAvatar from '@/components/AppItemListAvatar'
export default {
  props: ['data'],
  components: {
    'app-item-list-title': AppItemListTitle,
    'app-item-list-text': AppItemListText,
    'app-item-list-avatar': AppItemListAvatar
  },
  methods: {
    showTitle () {
      return this.$refs.title.showTitle()
    },
    showAvatar () {
      return this.$refs.avatar.showAvatar()
    },
    showDataCV () {
      return this.$refs.text.showText()
    }
  },
  data () {
    return {
      // dataTitle: null,
      // dataAvatar: null,
      // dataCV: null
    }
  },
  computed: {
    // dataTitle: function () {
    //   this.$refs.title.showTitle()
    // }
  },
  mounted () {
    // console.log('RRRTTT', this.$refs.title)
    // this.$refs.title.showTitle()
  }
}
</script>

<style scoped>

</style>
