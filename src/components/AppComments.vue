<template>
  <div class="card">
    <h2>Комментарии<sup class="supCount">{{ comments.length }}</sup></h2>

    <div>
      <ul class="list">
        <li
          class="list-item"
          v-for="comment in comments"
          :key="comment.id"
        >
          <div>
            <p><strong>{{ comment.email }}</strong></p>
            <small>{{ comment.body }}</small>
          </div>
        </li>
      </ul>
    </div>

  </div>
</template>

<script>

export default {
  props: ['comments']
}
</script>

<style scoped>
 .supCount {
   padding: 4px;
   background-color: #42b983;
   color: #ffffff;
   margin: 2px;
   border-radius: 3px;
   font-size: 8px;
 }
</style>
