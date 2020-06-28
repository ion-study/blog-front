<template>
  <div class="container">
    <h2 class="reg-tit">게시글 수정</h2>
    <div class="reg-contents">
      <div class="board-box">
        <table>
          <tr>
            <td>아이디</td>
            <td>{{postObj.userId}}</td>
          </tr>
          <tr>
            <td><label for="subject">제목</label></td>
            <td><input type="text" value="" name="subject" id="subject" v-model="newSubject"></td>
          </tr>
          <tr>
            <td><label for="contents">내용</label></td>
            <td><textarea name="contents" id="contents" v-model="newContent"></textarea></td>
          </tr>
        </table>
        <div class="button-wrap">
          <button type="button" @click="edit">저장</button>
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
    data () {
      return {
        postObj: {},
      }
    },
    computed: {
      newSubject () {
        return this.oriPost.subject
      },
      newContent () {
        return this.oriPost.contents
      }
    },
    methods: {
      edit () {
        console.log('edit')
        if (this.userId && this.subject && this.content) {
          this.$axios.$patch(`http://localhost:8080/boards/${this.$route.params.id}`, {
            subject: this.newSubject,
            contents: this.newContent
          }).then(()=>{
            console.log('success')
          })
        } else {
          alert('error')
        }
      }
    },
    created () {
      this.$axios.$get(`http://localhost:8080/boards/${this.$route.params.id}`).then(res=>{
        console.log(res)
        this.postObj = res
      })
    }
  }
</script>

<style>
  .reg-tit {
    text-align:left; padding:20px 0; border-bottom:1px solid #acacac;
  }
  .reg-contents {
    width:100%;
    margin:0 auto;
    padding :30px 0;

  }
  .board-box{
    width:100%;
    max-width:1024px;
    margin:0 auto;
  }
  .board-box table{
    width:100%;
  }
  .board-box table tr{
    border:1px solid #acacac;
  }
  .board-box table tr td{
    border:1px solid #acacac;
  }
  .board-box table tr td:first-child{
    width:120px;
  }
  .button-wrap{
    margin:10px 0;
  }

</style>
