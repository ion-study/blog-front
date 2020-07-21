<template>
  <div class="container">
    <div class="reg-contents">
      <b-card header="게시글 수정" class="mb-2 board-box" border-variant="info" align="left">
        <table>
          <tbody>
            <tr>
              <td>아이디</td>
              <td>{{ postObj.userId }}</td>
            </tr>
            <tr>
              <td>카테고리</td>
              <td>
                <select name="catId" id="catId" v-model="updateObj.catId">
                  <option v-for="(cat,idx) in catList" :value="cat.catId" :selected="idx===0">{{cat.catName}}</option>
                </select>
              </td>
            </tr>
            <tr>
              <td><label for="subject">제목</label></td>
              <td><input id="subject" v-model="updateObj.subject" type="text" value="" name="subject"></td>
            </tr>
            <tr>
              <td><label for="contents">내용</label></td>
              <td><textarea id="contents" v-model="updateObj.contents" name="contents"></textarea></td>
            </tr>
          </tbody>
        </table>
        <!-- 하단 버튼 -->

      </b-card>
      <div class="mt-3">
        <b-button variant="outline-primary" @click="edit">수정</b-button>
        <b-button variant="outline-danger" @click="$router.back()">취소</b-button>
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
    const catData = await app.$axios.$get(`categories?blogId=1`)
    // console.log(data)
    return {
      postObj: data,
      catList: catData
    }
  },
  data () {
    return {
      catList: [],
      postObj: {},
      updateObj: {
        boardId: '',
        subject: '',
        contents: '',
        catId: ''
      }
    }
  },
  created () {
    this.updateObj.boardId = this.postObj.boardId
    this.updateObj.subject = this.postObj.subject
    this.updateObj.contents = this.postObj.contents
    this.updateObj.catId = this.postObj.catId
  },
  methods: {
    edit () {
      this.$axios.$patch(`boards`, this.updateObj).then(() => {
        this.$router.push('/board')
      })
    }
  }
}
</script>

<style>

  .reg-contents {
    width: 100%;
    margin: 0 auto;
    padding: 30px 0;

  }

  .board-box {
    width: 100%;
    max-width: 80rem;
    margin: auto;
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
  #contents{
    width:100%;
    height:350px;
  }
  td{
    padding:0 20px;
  }
  td input {
    width: 100%;
  }


</style>
