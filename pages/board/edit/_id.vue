<template>
  <div class="container">
    <h2 class="reg-tit">
      게시글 수정 (미완)
    </h2>
    <div class="reg-contents">
      {{ postObj }}
      <br>
      <br>
      {{ updateObj }}
      <div class="board-box">
        <table>
          <tbody>
            <tr>
              <td>아이디</td>
              <td>{{ postObj.userId }}</td>
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
        <div class="mt-3">
          <b-button-group>
            <b-button variant="info" @click="edit">저장</b-button>
            <b-button variant="secondary" @click="$router.back()">취소</b-button>
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
    // console.log(data)
    return {
      postObj: data
    }
  },
  data () {
    return {
      postObj: {},
      updateObj: {
        subject: '',
        contents: ''
      }
    }
  },
  created () {
    this.updateObj.subject = this.postObj.subject
    this.updateObj.contents = this.postObj.contents
  },
  methods: {
    edit () {
      // 오류
      console.log('edit')
      this.$axios.$patch(`boards/${this.postObj.boardId}`, this.updateObj).then(() => {
        console.log('success')
        this.$router.push('/board')
      })
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
