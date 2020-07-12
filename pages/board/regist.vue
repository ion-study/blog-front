<template>
  <div class="container">
    <h2 class="reg-tit">
      게시글 등록
    </h2>
    <div class="reg-contents">
      <div class="board-box">
        <table>
          <tbody>
          <tr>
            <td><label for="userId">* 아이디</label></td>
            <td><input id="userId" v-model="registObj.userId" type="text" name="userId"></td>
          </tr>
          <tr>
            <td><label for="subject">* 제목</label></td>
            <td><input id="subject" v-model="registObj.subject" type="text" value="" name="subject"></td>
          </tr>
          <tr>
            <td><label for="contents">* 내용</label></td>
            <td><textarea id="contents" v-model="registObj.contents" name="contents"></textarea></td>
          </tr>
          </tbody>
        </table>
        <div class="mt-3">
          <b-btn-group>
            <b-button variant="primary" type="button" @click="regist">등록</b-button>
            <b-button type="button" @click="$router.back()">취소</b-button>
          </b-btn-group>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Regist',
  data () {
    return {
      registObj: {
        userId: '',
        subject: '',
        contents: ''
      }
    }
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
        console.log('regist')
        this.$axios.$post('boards', this.registObj).then(() => {
          console.log('success')
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
