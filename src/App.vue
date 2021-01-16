<template>
  <div class="container column">
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

      <button class="btn primary" :disabled="value.length < countEnableAddButton">Добавить</button>
    </form>

    <div class="card card-w70">
      <h3 v-if="!dataCV && !dataTitle && !dataAvatar">Добавьте первый блок, чтобы увидеть результат</h3>

      <div v-else>
        <h1>{{ dataTitle }}</h1>
        <div class="avatar">
          <img :src="dataAvatar" />
        </div>
        <div v-for="cv in dataCV" :key="cv.id">
          <h2 v-if="cv.type === 'subtitle'">{{ cv.type === 'subtitle' ? cv.value : ''}}</h2>
          <p v-if="cv.type === 'text'">
            {{ cv.type === 'text' ? cv.value : ''}}
          </p>
        </div>
      </div>
    </div>
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
import axios from 'axios'

export default {
  components: {
    AppComments,
    AppLoader
  },
  mounted () {
    this.loadData()
  },
  data () {
    return {
      loadingState: false,
      comments: [],
      alertMessage: null,
      clickLoadComments: false,
      type: 'title',
      value: '',
      countEnableAddButton: 4,
      data: [],
      dataTitle: null,
      dataAvatar: null,
      dataCV: null
    }
  },
  methods: {
    showTitle () {
      const title = this.data.filter(element => element.type === 'title')
      if (title.length > 0) {
        this.dataTitle = title[0].value
      } else {
        this.dataTitle = null
      }
    },
    showAvatar () {
      const avatar = this.data.filter(element => element.type === 'avatar')
      if (avatar.length > 0) {
        this.dataAvatar = avatar[0].value
      } else {
        this.dataAvatar = null
      }
    },
    showData () {
      this.dataCV = this.data.filter(element => (element.type === 'subtitle' || element.type === 'text'))
    },
    async deleteData (id) {
      await axios.delete(`https://vue-with-http-1da46-default-rtdb.firebaseio.com/cv/${id}.json`)
    },
    async createData () {
      if (this.type === 'title' || this.type === 'avatar') {
        const oldTitle = this.data.find(element => element.type === 'title')
        if (oldTitle) {
          await this.deleteData(oldTitle.id)
        }
      }

      const url = 'https://vue-with-http-1da46-default-rtdb.firebaseio.com/cv.json'

      const response = await fetch(url, {
        method: 'POST',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify({
          type: this.type,
          value: this.value
        })
      })

      const firebaseData = await response.json()

      this.data.push({
        type: this.type,
        value: this.value,
        id: firebaseData.name
      })

      this.type = 'title'
      this.value = ''

      await this.loadData()
    },
    async loadData () {
      try {
        const url = 'https://vue-with-http-1da46-default-rtdb.firebaseio.com/cv.json'

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
      this.showTitle()
      this.showAvatar()
      this.showData()
    },
    async loadComments () {
      this.clickLoadComments = true
      try {
        this.loadingState = true
        const url = 'https://jsonplaceholder.typicode.com/comments?_limit=42'

        const { data } = await axios.get(url)
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
      }
    }
  }
}
</script>

<style>
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
