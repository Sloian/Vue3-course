<template lang="">
  <div class="post">
    <div>
      <div><strong>Назва поста:</strong> {{post.title}}</div>
      <div><strong>Опис посту:</strong> {{post.body}}</div>
    </div>
    <div class="post_btns">
      <my-button @click="$emit('remove', post)">
      Видалити
      </my-button>
    </div>
  </div>
</template>


<script>

export default {

  props:{
    post:{
      type: Object,
      required: true,
    }
  }
}
</script>


<style scoped>

  .post{
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px;
    border: 2px solid lightseagreen;
    margin-top: 15px;
  }



</style>
