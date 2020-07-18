<template>
  <div class="container">
    <b-card border-variant="info" header="카테고리 관리" no-body class="box-wrap overflow-hidden">
      <b-row no-gutters>
        <b-col md="6">
          <b-card-body title="카테고리 목록" class="c-body-wrap1">
            <b-card-text class="card-text1">
              <p>* 항목 클릭 시, 우측 카테고리 상세 설정 영역 노출</p>
              <ul class="cat-list">
                <li v-for="cat in catList" :key="cat.catId" @click="selCat(cat)">{{cat.catName}}</li>
                <li><b-button variant="success" @click="selCat()">+ 메뉴 추가</b-button></li>
              </ul>
            </b-card-text>
          </b-card-body>
        </b-col>
        <b-col md="6">
          <b-card-body title="카테고리 상세 설정영역" class="c-body-wrap2">
            <b-card-text class="card-text2">
              <div class="board-box">
                <template v-if="tempCat">
                  <table>
                    <tbody>
                    <tr>
                      <td>상위 카테고리 선택</td>
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
                          <b-button variant="success" @click="updateCat">수정</b-button>
                        </b-btn-group>
                      </template>
                      <tempate v-else-if="setFlag==='REG'">
                        <b-button variant="outline-secondary" @click="initData">취소</b-button>
                        <b-button variant="outline-success" @click="registCat">추가</b-button>
                      </tempate>
                    </b-btn-group>
                  </div>
                </template>
                <div v-else>
                  * 수정 할 카테고리를 선택하세요.
                </div>
              </div>

            </b-card-text>
          </b-card-body>
        </b-col>
      </b-row>



    </b-card>
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
        if (this.tempCat.orderNum < 1 || !this.tempCat.orderNum) this.tempCat.orderNum = 1
        this.$axios.$get(`categories/${this.tempCat.parentCatId}`).then((parentCat)=>{
          this.tempCat.depth = parentCat.depth + 1
          this.$axios.$post('categories', this.tempCat).then(this.initData)
        })
      },
      updateCat () {
        if (this.tempCat.orderNum < 1 || !this.tempCat.orderNum) this.tempCat.orderNum = 1
        this.$axios.$get(`categories/${this.tempCat.parentCatId}`).then((parentCat)=>{
          this.tempCat.depth = parentCat.depth + 1
          this.$axios.$patch('categories', this.tempCat).then(this.initData)
        })
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
  .box-wrap{
    width: 100%;
    max-width: 80rem;
    margin: auto;
    padding-bottom:20px;
  }
  .c-body-wrap1{
    margin-top:10px;
    border-right: 1px solid #acacac;
    height: 100%;
  }
  .c-body-wrap1 .card-text1{
    margin:50px 0;
  }
  .c-body-wrap1 .card-text1 ul{
    padding-left:0;
  }
  .c-body-wrap2{
    margin-top:10px;
  }
  .c-body-wrap2 .card-text2{
    margin:50px 0;
  }
  table{
    margin:0 auto;
    width:100%;
  }
  tbody tr td{
    text-align: left;
    padding:0 20px;
  }
  tbody tr td:nth-child(1) {
    border-right: 1px solid #acacac;
  }
  tbody tr td:nth-child(2) select{
    width:100%;
  }
  tbody tr td:nth-child(2) input{
    width:100%;
  }

  @media (max-width:767px) {
    .c-body-wrap1{
      border-right: none;
      border-bottom: 1px solid #acacac;
    }
    .c-body-wrap2{
      margin-top:20px;
    }
    .c-body-wrap1 .card-text1{
      margin:0;
    }
    .c-body-wrap2 .card-text2{
      margin:0;
    }
  }
</style>
