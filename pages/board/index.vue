<template>
  <div>
    <h1>게시글 리스트</h1>
    <div>
      <ul>
        <li v-for="post in boardList" :key="post.boardId"><nLink :to="`board/view/${post.boardId}`">[{{post.boardId}}] / {{post.userId}} / {{post.subject}} / {{post.createdDate}}</nLink></li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    name: "index",
    async asyncData ({ app }) {
      let data  = await app.$axios.$get('boards')
      return {
        boardList: data
      }
    },
    data () {
      return {
        boardList:[]
      }
    }
  }
</script>

<style scoped>

</style>
