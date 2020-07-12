<template>
  <div class="container">
    <h2 class="reg-tit">
      카테고리 생성
    </h2>
    <div class="reg-contents">
      <div class="board-box">
        <table>
          <tr>
            <td><label for="parentCatId">상위카테고리 ID</label></td>
            <td><input id="parentCatId" v-model="registObj.parentCatId" type="text" name="parentCatId"></td>
          </tr>
          <tr>
            <td><label for="catName">카테고리 이름</label></td>
            <td><input id="catName" v-model="registObj.catName" type="text" name="catName"></td>
          </tr>
          <tr>
            <td><label for="orderNum">정렬</label></td>
            <td><input id="orderNum" v-model="registObj.orderNum" type="text" name="orderNum"></td>
          </tr>
          <tr>
            <td><label for="depth">뎁스</label></td>
            <td><input id="depth" v-model="registObj.depth" type="number" name="depth"></td>
          </tr>
          <tr>
            <td><label for="blogId">블로그ID</label></td>
            <td><input id="blogId" v-model="registObj.blogId" type="number" name="blogId" disabled></td>
          </tr>
        </table>
        <div class="mt-3">
          <b-btn-group>
            <b-button variant="primary" @click="regist">확인</b-button>
            <b-button @click="$router.back()">취소</b-button>
          </b-btn-group>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
  export default {
    data () {
      return {
        registObj: {
          parentCatId: '',
          catName: '',
          orderNum: '1', // 기본값
          depth: '1', // 기본값
          blogId: 1, // 블로그ID 고정값 (추후 개발 예정)
        }
      }
    },
    methods: {
      regist () {
        this.$axios.$post('categories', this.registObj).then(()=>{
          alert('카테고리가 생성되었습니다.')
          this.$router.push('/')
        })
      }
    }
  }
</script>
<style>
  .reg-contents {
    width: 100%;
    margin: 0 auto;
    border: 1px solid #acacac;
    padding: 30px 0;
  }

  .reg-contents table {
    border: 1px solid #acacac;
    border-collapse: collapse;
    width: 960px;

    margin: 0 auto;
  }

  .reg-contents table tr td {
    border: 1px solid #acacac;
  }
</style>
