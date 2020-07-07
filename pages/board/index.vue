<template>
  <div>
    <h1>게시글 리스트</h1>
    <div>
      <ul>
        <li v-for="post in boardList" :key="post.boardId">[{{post.boardId}}] / {{post.userId}} / {{post.subject}} / {{post.createdDate}}<nLink :to="`board/view/${post.boardId}`"></nLink><button v-on:click="updateTest(post.boardId)">[삭제]</button></li>
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
    },
    methods: {
      async updateTest(id) {
        //let data2 = await this.$axios({method: 'delete', url:`boards/${id}`}) // delete test
        //let data2 = await this.$axios.$patch(`boards/${id}`, {"subject": "axios subject수정", "contents": "axios content수정"}); // update test
        //console.log(data2)
      }
    }
  }
</script>

<style scoped>

</style>
