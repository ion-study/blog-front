<template>
  <div>
    <h1>게시글 리스트</h1>
    <div>
      <ul>
        <li v-for="post in boardList" :key="post.id">{{post.userId}} : <nLink :to="`board/view/${post.id}`"></nLink>{{post.title}} / {{post.createdDate}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    name: "index",
    asyncData () {
      let data = this.$axios.$get('http://localhost:8080/boards/').then(res=>{
        console.log(res)
      })
      return {
        boardList: data
      }
    },
    data () {
      return {
        boardList: []
      }
    }
  }
</script>

<style scoped>

</style>
