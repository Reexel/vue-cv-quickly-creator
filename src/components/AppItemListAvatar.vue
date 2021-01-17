<template>
  <div class="avatar">
    <img :src="dataAvatar" />
  </div>
</template>

<script>
export default {
  props: ['data'],
  methods: {
    showAvatar () {
      return this.dataAvatar
    }
  },
  computed: {
    dataAvatar: function (data) {
      if (data.data.length > 0) {
        const avatar = data.data.filter(element => element.type === 'avatar')
        if (avatar.length > 0) {
          return avatar[0].value
        }
      }
      return null
    }
  }
}
</script>

<style scoped>
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }
</style>
