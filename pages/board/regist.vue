<template>
  <div class="container">
    <div class="reg-contents">
      <b-card header="게시글 등록" class="mb-2 board-box" border-variant="info" align="left">
        <table>
          <tbody>
          <tr>
            <td><label for="userId">* 아이디</label></td>
            <td><input id="userId" v-model="registObj.userId" type="text" name="userId"></td>
          </tr>
          <tr>
            <td><label for="userId">* 카테고리 선택</label></td>
            <td>
              <select name="catId" id="catId" v-model="registObj.catId">
                <option v-for="(cat,idx) in catList" :value="cat.catId" :selected="idx===0">{{cat.catName}}</option>
              </select>
            </td>
          </tr>
          <tr>
            <td><label for="subject">* 제목</label></td>
            <td><input id="subject" v-model="registObj.subject" type="text" name="subject"></td>
          </tr>
          <tr>
            <td><label for="contents">* 내용</label></td>
            <td><textarea id="contents" v-model="registObj.contents" name="contents"></textarea></td>
          </tr>
          </tbody>
        </table>
      </b-card>
      <div class="mt-3">
        <b-button variant="outline-success" type="button" @click="regist">등록</b-button>
        <b-button variant="outline-danger" type="button" @click="$router.back()">취소</b-button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  async asyncData ({ app, params }) {
    const catData = await app.$axios.$get(`categories?blogId=1`)
    return {
      catList: catData
    }
  },
  data () {
    return {
      catList: [],
      registObj: {
        userId: '',
        catId:'',
        subject: '',
        contents: '',
      }
    }
  },
  mounted () {
    this.registObj.catId = this.catList[0].catId
  },
  methods: {
    validate () {
      if (!this.registObj.userId || !this.registObj.subject || !this.registObj.contents) {
        alert('필수 입력값(*)을 모두 입력하세요.')
        return false
      }
      return true
    },
    regist () {
      if (this.validate()) {
        this.$axios.$post('boards', this.registObj).then(() => {
          this.$router.push(`/board/${this.registObj.catId}`)
        })
      }
    }
  }
}
</script>
<style scoped>

  .container{
    display: inline-block;
    width: 100%;
    max-width: 850px;
  }

  .reg-contents {
    width: 100%;
    margin: 0 auto;
    padding: 30px 0
  }

  .board-box {
    width: 100%;
    max-width: 80rem;
    margin: auto;
  }

  .board-box table {
    width: 100%;
  }

  .board-box table tr td {
    border: 1px solid #acacac;
  }

  .board-box table tr td:first-child {
    width: 170px;
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
