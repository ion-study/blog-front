<template>
  <div>
    <h1>게시글 리스트</h1>
    <div>
      <ul>
        <li v-for="post in boardList" :key="post.id">[{{post.boardId}}] / {{post.userId}} / {{post.subject}} / {{post.createdDate}}<nLink :to="`board/view/${post.id}`"></nLink></li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    name: "index",
    async asyncData ({ app }) {
      let data  = await app.$axios.$get('boards')
      console.log(data)
      return {
        boardList: data
      }
    }
  }
</script>

<style scoped>

</style>
