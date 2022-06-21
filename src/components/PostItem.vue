<template>
  <div class="post">
    <div>
      <div>{{ post.id }}</div>
      <div><strong>Название:</strong> {{ post.title }}</div>
      <div><strong>Описание:</strong> {{ post.body }}</div>
    </div>
    <div class="post__buttons">
      <my-button @click="$emit('remove', post)">Удалить</my-button>
    </div>
  </div>
</template>

<script>

export default {
  name: "PostItem",
  props: {
    post: {
      type: Object,
      require: true
    }
  }
}
</script>

<style scoped>
  .post {
    padding: 15px;
    border: 2px solid #3A68B8;
    margin-top: 15px;
    border-radius: 5px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
</style>