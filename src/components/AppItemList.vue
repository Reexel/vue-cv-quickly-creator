<template>
  <div class="card card-w70">
    <h3 v-if="$props.data.length === 0">Добавьте первый блок, чтобы увидеть результат</h3>

    <div v-else>
      <component
        :is="componentName(cv.type)"
        v-for="cv in $props.data"
        :text="cv.text"
        :key="cv.id"
      ></component>

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
    // showTitle () {
    //   return this.$refs.title.showTitle()
    // },
    // showAvatar () {
    //   return this.$refs.avatar.showAvatar()
    // },
    // showDataCV () {
    //   return this.$refs.text.showText()
    // },
    componentName (name) {
      let compName = ''
      switch (name) {
        case ('title'):
          compName = 'AppItemListTitle'
          break
        case ('avatar'):
          compName = 'AppItemListAvatar'
          break
        case ('subtitle'):
          compName = 'AppItemListSubtitle'
          break
        case ('text'):
          compName = 'AppItemListText'
          break
      }
      return compName
    }
  },
  data () {
    return {
      // dataTitle: null,
      // dataAvatar: null,
      // dataCV: null
      // data: this.data
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
