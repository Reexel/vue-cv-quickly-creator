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
      await axios.post(process.env.VUE_APP_URL_DATA + '/' + '.json', params)
    },
    async createData (value, type) {
      const params = JSON.stringify({
        type: type,
        value: value
      })

      if (type === 'title' || type === 'avatar') {
        const element = this.data.find(element => element.type === type)
        if (element) {
          await this.updateData(element.id, params)
        } else {
          await this.insertData(params)
        }
      } else {
        await this.insertData(params)
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
