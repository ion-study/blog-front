<template>
  <div class="container">
    <h2 class="reg-tit">게시글 등록</h2>
    <div class="reg-contents">
      <div class="board-box">
        <table>
          <tr>
            <td><label for="userId">아이디</label></td>
            <td><input type="text" name="userId" id="userId" v-model="userId"></td>
          </tr>
          <tr>
            <td><label for="subject">제목</label></td>
            <td><input type="text" value="" name="subject" id="subject" v-model="subject"></td>
          </tr>
          <tr>
            <td><label for="contents">내용</label></td>
            <td><textarea name="contents" id="contents" v-model="content"></textarea></td>
          </tr>
        </table>
        <div class="button-wrap">
          <button type="button" @click="post">등록</button>
        </div>
      </div>
    </div>
  </div>

</template>
<script>
  export default {
    name: 'regist',
    data () {
      return {
        userId: '',
        subject: '',
        content: ''
      }
    },
    methods: {
      post () {
        console.log('post')
        if (this.userId && this.subject && this.content) {
          this.$axios.$post(`http://localhost:8080/boards/`, {
            userId: this.userId,
            subject: this.subject,
            contents: this.content
          }).then(()=>{
            console.log('success')
          })
        } else {
          alert('error')
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
