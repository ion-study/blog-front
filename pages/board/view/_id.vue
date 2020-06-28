<template>
  <div class="container">
    <h2 class="reg-tit">게시글 조회</h2>
    <div class="reg-contents">
      <div class="board-box">
        <table>
          <tbody>
          <tr>
            <td>게시글 ID (삭제예정)</td>
            <td>{{$route.params.id}}</td>
          </tr>
          <tr>
            <td>아이디</td>
            <td id="userId"></td>
          </tr>
          <tr>
            <td>제목</td>
            <td id="subject"></td>
          </tr>
          <tr>
            <td>내용</td>
            <td id="contents"></td>
          </tr>
          </tbody>
        </table>

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
        postObj: {}
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
