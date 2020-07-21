<template>
  <div class="container">
    <h2 class="reg-tit">
      게시글 조회
    </h2>
    <div class="reg-contents">
      <div class="board-box">
        <table>
          <tbody>
            <tr>
              <td>게시글 ID (삭제예정)</td>
              <td>{{ postObj.boardId }}</td>
            </tr>
            <tr>
              <td>아이디</td>
              <td>{{ postObj.userId }}</td>
            </tr>
            <tr>
              <td>카테고리</td>
              <td>{{ catName }}</td>
            </tr>
            <tr>
              <td>제목</td>
              <td>{{ postObj.subject }}</td>
            </tr>
            <tr>
              <td>내용</td>
              <td>{{ postObj.contents }}</td>
            </tr>
          </tbody>
        </table>
        <!-- 하단 버튼 -->
        <div class="mt-3">
          <b-button-group>
            <b-button variant="info" @click="editPost">수정</b-button>
            <b-button variant="warning" @click="deletePost">삭제</b-button>
            <b-button variant="secondary" @click="$router.back()">뒤로</b-button>
          </b-button-group>
        </div>
      </div>
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
    const data = await app.$axios.$get(`boards/${params.id}`)
    const cat = await app.$axios.$get(`categories/${data.catId}`)

    // console.log(data)
    return {
      postObj: data,
      catName: cat.catName
    }
  },
  data () {
    return {
      postObj: {},
      catName: ''
    }
  },
  methods: {
    editPost () {
      this.$router.push(`/board/edit/${this.postObj.boardId}`)
    },
    deletePost () {
      let deleteConform = confirm("게시글을 삭제하시겠습니까?")
      if(deleteConform)  {
        this.$axios.$delete(`boards/${this.postObj.boardId}`).then(()=>{
          alert("게시글이 성공적으로 삭제되었습니다. 목록으로 이동합니다.")
          this.$router.push('/board')
        })
      }
    }
  }
}
</script>

<style>
  .reg-tit {
    text-align: left;
    padding: 20px 0;
    border-bottom: 1px solid #acacac;
  }

  .reg-contents {
    width: 100%;
    margin: 0 auto;
    padding: 30px 0;

  }

  .board-box {
    width: 100%;
    max-width: 1024px;
    margin: 0 auto;
  }

  .board-box table {
    width: 100%;
  }

  .board-box table tr {
    border: 1px solid #acacac;
  }

  .board-box table tr td {
    border: 1px solid #acacac;
  }

  .board-box table tr td:first-child {
    width: 120px;
  }

  .button-wrap {
    margin: 10px 0;
  }

</style>
