<template>
  <div>
    <div class="reg-contents">
      <b-card header="게시글 리스트" class="mb-2 board-box" border-variant="info" align="left">
        <ul>
          <li v-for="post in boardList" :key="post.boardId">
            <nLink :to="`board/view/${post.boardId}`">
              [{{ post.boardId }}] / {{ post.userId }} / {{ post.subject }} / {{ post.createdDate }}
            </nLink>

          </li>
        </ul>
      </b-card>
      <div class="mt-3">
        <b-button variant="outline-danger" @click="deletePost(post.boardId)">삭제</b-button>
        <b-button variant="outline-primary" @click="$router.push(`/board/edit/${post.boardId}`)">수정</b-button>
      </div>
    </div>
    <b-button-group>
      <b-button variant="primary" @click="$router.push('/board/regist')">글쓰기</b-button>
    </b-button-group>
  </div>
</template>

<script>
  export default {
    name: 'Index',
    async asyncData ({ app }) {
      const data = await app.$axios.$get('boards')
      console.log(data)
      return {
        boardList: data
      }
    },
    data () {
      return {
        boardList: []
      }
    },
    methods: {
      updateTest (id) {
        let bodyData = {
          "boardId": id,
          "subject": "axios subject수정",
          "contents": "axios content수정"
        }
        this.$axios.$patch(`boards`, bodyData).then(()=>{
          console.log(bodyData)
        })
      },
      deletePost (boardId) {
        let deleteConform = confirm("게시글을 삭제하시겠습니까?")
        if(deleteConform)  {
          this.$axios.$delete(`boards/${boardId}`).then(()=>{
            alert("게시글이 성공적으로 삭제되었습니다.")
            this.$axios.$get('boards').then((res)=>{
              this.boardList = res
            })
          })
        }
      }
    }
  }
</script>

<style scoped>

</style>
