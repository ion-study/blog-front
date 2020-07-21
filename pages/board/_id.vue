<template>
  <div class="container">
    <div class="reg-contents">
      <b-card :header="cat.catName" class="mb-2 board-box" border-variant="info" align="left">
        <p v-if="boardList.length === 0">게시글이 없습니다.</p>
        <table v-else>
          <tr v-for="post in boardList" :key="post.boardId">
            <td class="td-a">
            <nLink :to="`/board/view/${post.boardId}`">
              [{{ post.boardId }}] / {{ post.userId }} / {{ post.subject }} / {{ post.createdDate }}
            </nLink>
            </td>
            <td class="td-b">
            <span class="ml-3 btn-wrap">
              <b-button variant="outline-danger" @click="deletePost(post.boardId)">삭제</b-button>
              <b-button variant="outline-primary" @click="$router.push(`/board/edit/${post.boardId}`)">수정</b-button>
            </span>
            </td>
          </tr>
        </table>
      </b-card>
      <div class="mt-3">
      </div>
    </div>
    <div class="mt-3">
      <b-button variant="primary" @click="$router.push('/board/regist')">글쓰기</b-button>
    </div>
  </div>
</template>

<script>
  export default {
    validate ({ params }) {
      // Must be a number
      return /^\d+$/.test(params.id)
    },
    async asyncData ({ app, params }) {
      const boardData = await app.$axios.$get('boards')
      const cat = await app.$axios.$get(`categories/${params.id}`)
      return {
        cat: cat,
        boardList: boardData.filter((item => item.catId == params.id))
      }
    },
    data () {
      return {
        catObj: {},
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
          // console.log(bodyData)
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
  .td-a{
    width:80%;
    border:1px solid #acacac;
  }
  .td-b{
    width:20%;
  }

  .container{
    display: inline-block;
    width: 100%;
    max-width: 850px;
    margin-top:30px;
  }

</style>
