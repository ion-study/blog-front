<template>
  <div class="container">
    <h2 class="reg-tit">카테고리 관리</h2>
    <hr>
    <h3>카테고리 목록</h3>
    <p>* 항목 클릭 시, 하단 카테고리 설정영역 replace</p>
    <ul class="cat-list">
      <li v-for="cat in catList" :key="cat.catId" @click="selCat(cat)">{{cat.catName}}</li>
      <li><b-button variant="info" @click="selCat()">+ 메뉴 추가</b-button></li>
    </ul>
    <hr>
    <h3>카테고리 상세 설정영역</h3>
    <div class="reg-contents">
      <div class="board-box">
        <template v-if="tempCat">
        <table>
          <tbody>
          <tr>
            <td>상위카테고리 선택</td>
            <td>
              <select v-model="tempCat.parentCatId">
                <option :value="0">선택 안함</option>
                <option :value="cat.catId" v-for="cat in catList" :disabled="cat.catId === tempCat.catId">{{cat.catName}}</option>
              </select>
            </td>
          </tr>
          <tr>
            <td><label for="catName">카테고리 이름</label></td>
            <td><input id="catName" v-model="tempCat.catName" type="text" name="catName"></td>
          </tr>
          <tr>
            <td><label for="orderNum">정렬</label></td>
            <td><input id="orderNum" v-model="tempCat.orderNum" type="text" name="orderNum"></td>
          </tr>
          <tr>
            <td><label for="depth">뎁스</label></td>
            <td><input id="depth" v-model="tempCat.depth" type="number" name="depth"></td>
          </tr>
          <tr>
            <td><label for="blogId">블로그ID</label></td>
            <td><input id="blogId" v-model="tempCat.blogId" type="number" name="blogId" disabled></td>
          </tr>
          </tbody>
        </table>
          <div class="mt-3">
          <b-btn-group>
            <template v-if="setFlag==='MOD'">
              <b-btn-group>
                <b-button @click="initData">취소</b-button>
                <b-button variant="danger" @click="deletCat">삭제</b-button>
                <b-button variant="primary" @click="updateCat">수정</b-button>
              </b-btn-group>
            </template>
            <tempate v-else-if="setFlag==='REG'">
              <b-button @click="initData">취소</b-button>
              <b-button variant="primary" @click="registCat">추가</b-button>
            </tempate>
          </b-btn-group>
        </div>
        </template>
        <div v-else>
          수정 할 카테고리를 선택하세요.
        </div>
      </div>
    </div>

  </div>
</template>
<script>
  import lnb from '~/components/common/lnb'
  export default {
    components: {lnb},
    async asyncData ({app}) {
      const data = await app.$axios.$get('categories?blogId=1') // 아직 블로그id 관리 X
      // console.log(data)
      return {
        catList: data,
        setFlag: 'MOD'
      }
    },
    data () {
      return {
        catList: [],
        tempCat: null,
        parentCatOpts: []
      }
    },
    methods: {
      initData () {
        this.$axios.$get('categories?blogId=1').then((res)=>{
          this.catList = res
          this.tempCat = null
        })
      },
      selCat (cat) {
        if (cat) {
          this.tempCat = {...cat}
          this.setFlag = 'MOD'
        } else {
          this.tempCat = {
            parentCatId: 0,
            catName: '',
            orderNum: 1, // 기본값
            depth: 1, // 기본값
            blogId: 1, // 블로그ID 고정값 (추후 개발 예정)
          }
          this.setFlag = 'REG'
        }
      },
      registCat () {
        this.$axios.$post('categories', this.registCat).then(this.initData)
      },
      updateCat () {
        let bodyData = {
          "catId": this.tempCat.catId,
          "catName": this.tempCat.catName,
          "orderNum": this.tempCat.orderNum,
          "depth": this.tempCat.depth,
          "blogId": this.tempCat.blogId,
          "parentCatId":  this.tempCat.parentCatId
        }
        this.$axios.$patch('categories', bodyData).then(this.initData)
      },
      deletCat () {
        let deleteConform = confirm("해당 카테고리를 삭제 하시겠습니까?")
        if (deleteConform) {
          this.$axios.$delete(`categories/${this.tempCat.catId}?blogId=1`).then(this.initData)
        }
      }
    }
  }
</script>
<style scoped>

</style>
