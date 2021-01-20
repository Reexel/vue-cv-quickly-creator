<template>
  <div class="container column">
    <app-form @add-data="createData"></app-form>
    <app-item-list :data="data"></app-item-list>
  </div>
  <div class="container">
    <p>
      <button class="btn primary" @click="loadComments">Загрузить комментарии</button>
    </p>

    <app-loader v-if="loadingState"></app-loader>
    <app-comments
      v-else-if="clickLoadComments"
      :comments="comments"
      @load="loadComments"
    ></app-comments>
  </div>
</template>

<script>
import AppComments from '@/components/AppComments'
import AppLoader from '@/components/AppLoader'
import AppForm from '@/components/AppForm'
import AppItemList from '@/components/AppItemList'

import axios from 'axios'

export default {
  components: {
    AppComments,
    AppLoader,
    AppForm,
    AppItemList
  },
  mounted () {
    this.loadData()
  },
  data () {
    return {
      loadingState: false,
      comments: [],
      clickLoadComments: false,
      data: []
    }
  },
  methods: {
    async updateData (id, params) {
      await axios.put(process.env.VUE_APP_URL_DATA + '/' + id + '.json', params)
    },
    async insertData (params) {
      await axios.post(process.env.VUE_APP_URL_DATA + '.json', params)
    },
    async deleteData (id) {
      await axios.delete(process.env.VUE_APP_URL_DATA + '/' + id + '.json')
    },
    async createData (value, type) {
      const params = JSON.stringify({
        type: type,
        value: value
      })

      if (type === 'title') {
        const oldTitle = this.data.find(element => element.type === 'title')
        if (oldTitle) {
          await this.updateData(oldTitle.id, params)
        }
      } else if (type === 'avatar') {
        const oldAvatar = this.data.find(element => element.type === 'avatar')
        if (oldAvatar) {
          await this.updateData(oldAvatar.id, params)
        }
      } else {
        const url = process.env.URL_DATA + '.json'

        const response = await fetch(url, {
          method: 'POST',
          headers: {
            'Content-type': 'application/json'
          },
          body: JSON.stringify({
            type: type,
            value: value
          })
        })

        const firebaseData = await response.json()

        this.data.push({
          type: type,
          value: value,
          id: firebaseData.name
        })
      }

      await this.loadData()
    },
    async loadData () {
      try {
        const url = process.env.VUE_APP_URL_DATA + '.json'

        const { data } = await axios.get(url)
        if (!data) {
          throw new Error('Добавьте первый блок, чтобы увидеть результат')
        }
        this.data = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
          }
        })
      } catch (e) {
        this.alert = {
          type: 'danger',
          title: 'Ошибка загрузки',
          text: 'Ой! Что-то пошло не так. Попробуйте позже.'
        }
      }
    },
    async loadComments () {
      this.clickLoadComments = true
      try {
        this.loadingState = true
        const { data } = await axios.get(process.env.VUE_APP_URL_COMMENTS)
        if (!data) {
          throw new Error('Комментарии отсутствуют')
        }
        this.comments = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
          }
        })
        this.loadingState = false
      } catch (e) {
        this.alert = {
          type: 'danger',
          title: 'Ошибка загрузки',
          text: 'Ой! Что-то пошло не так. Попробуйте позже.'
        }
        this.loadingState = false
        this.clickLoadComments = false
      }
    }
  }
}
</script>

<style>

</style>
