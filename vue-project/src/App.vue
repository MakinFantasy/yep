<template>
  <div>
    <div class="post">
      <div><strong>Название:</strong> Пост о JS</div>
      <div><strong>Название:</strong> Пост о JS</div>
    </div>
  </div>

</template>

<script>
export default {
  data() {
    return {
      likes: 0,
      dislikes: 3,
    }
  },


  
  methods: {
    addLike() {
      this.likes += 1;
    },


    addDislike() {
      this.dislikes += 1;
    },
  }
}
</script>

<style>
* {
  margin: 0; 
  padding: 0;
  box-sizing: border-box;
}

.post {
  padding: 15px;
  border: 1px
}

</style>