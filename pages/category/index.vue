<template>
  <div class="container">
    <b-card border-variant="info" header="카테고리 관리" no-body class="box-wrap overflow-hidden">
      <b-row no-gutters>
        <b-col md="6">
          <b-card-body title="카테고리 목록" class="c-body-wrap1">
            <b-card-text class="card-text1">
              <lnb setup :catList="catList"
                   @sel-cat="selCat" />
              <b-button variant="success" class="mt-3" @click="selCat()" >+ 메뉴 추가</b-button>
            </b-card-text>
          </b-card-body>
        </b-col>
        <b-col md="6">
          <b-card-body title="카테고리 상세 설정영역" class="c-body-wrap2">
            <b-card-text class="card-text2">
              <div class="board-box">
                <template v-if="tempCat">
                  <p v-if="setFlag==='MOD'">* 수정을 원하는 카테고리 내용을 변경하세요.</p>
                  <p v-if="setFlag==='REG'">* 등록을 원하는 카테고리 내용을 입력하세요.</p>
                  <table>
                    <tbody>
                    <tr>
                      <td>상위 카테고리 선택</td>
                      <td>
                        <p v-if="tempCat.child">상위카테고리 지정 불가 <br><small>하위카테고리가 있을 경우 상위카테고리를 지정 할 수 없습니다.</small></p>
                        <select v-else v-model="tempCat.parentCatId">
                          <option :value="0">선택 안함</option>
                          <option :value="cat.catId" v-for="cat in catList" v-if="cat.depth==0" :disabled="cat.catId === tempCat.catId">{{cat.catName}}</option>
                        </select>
                      </td>
                    </tr>
                    <tr>
                      <td><label for="catName">* 카테고리 이름</label></td>
                      <td><input id="catName" v-model="tempCat.catName" type="text" name="catName"></td>
                    </tr>
                    <tr>
                      <td><label for="orderNum">* 정렬</label></td>
                      <td><input id="orderNum" v-model="tempCat.orderNum" type="number" min="0" name="orderNum"></td>
                    </tr>
<!--                    <tr>-->
<!--                      <td><label for="depth">뎁스</label></td>-->
<!--                      <td><input id="depth" v-model="tempCat.depth" type="number" name="depth" disabled></td>-->
<!--                    </tr>-->
<!--                    <tr>-->
<!--                      <td><label for="blogId">블로그ID</label></td>-->
<!--                      <td><input id="blogId" v-model="tempCat.blogId" type="number" name="blogId" disabled></td>-->
<!--                    </tr>-->
                    </tbody>
                  </table>
                  <div class="mt-3">
                      <template v-if="setFlag==='MOD'">
                        <b-button varient="secondary" @click="initData">취소</b-button>
                        <b-button variant="danger" @click="deleteCat">삭제</b-button>
                        <b-button variant="success" @click="updateCat">수정</b-button>
                      </template>
                      <template v-else-if="setFlag==='REG'">
                        <b-button variant="secondary" @click="initData">취소</b-button>
                        <b-button variant="success" @click="registCat">등록</b-button>
                      </template>
                  </div>
                </template>
                <div v-else class="dfault-txt">
                  * 메뉴 추가 버튼 또는 리스트 항목 클릭 시, <br>카테고리 상세 설정 영역 활성화
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
      return {
        catList: data,
        setFlag: 'MOD'
      }
    },
    data () {
      return {
        catList: [],
        tempCat: null,
        setFlag: 'MOD'
      }
    },
    computed: {
      bodyData () {
        if (this.tempCat) delete this.tempCat.child
        return this.tempCat
      }
    },
    methods: {
      initData () {
        this.$axios.$get('categories?blogId=1').then((data)=>{
          this.catList = data
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
            depth: 0, // 기본값
            blogId: 1, // 블로그ID 고정값 (추후 개발 예정)
          }
          this.setFlag = 'REG'
        }
      },
      validate () {
        if (!this.bodyData.catName) {
          alert('필수 입력값(*)을 모두 입력하세요.')
          return false
        }
        return true
      },
      registCat () {
        if (this.validate()){
          this.$axios.$get(`categories/${this.tempCat.parentCatId}`).then((parentCat)=>{
            this.tempCat.depth = parentCat.catId==0||!parentCat.catId ? 0 : parentCat.depth + 1
            this.$axios.$post('categories', this.bodyData).then(this.initData)
          })
        }
      },
      updateCat () {
        if (this.validate()){
          this.$axios.$get(`categories/${this.tempCat.parentCatId}`).then((parentCat)=>{
            this.tempCat.depth = parentCat.catId==0||!parentCat.catId ? 0 : parentCat.depth + 1
            this.$axios.$patch('categories', this.bodyData).then(this.initData)
          })
        }
      },
      deleteCat () {
        let deleteConform = confirm("해당 카테고리를 삭제 하시겠습니까?")
        if (deleteConform) {
          this.$axios.$delete(`categories/${this.tempCat.catId}?blogId=1`)
            .then(this.initData)
            .catch((err)=>{
              console.log(err.status)
            })
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
    margin:30px 0;
  }
  .c-body-wrap1 .card-text1 ul{
    padding-left:0;
  }
  .c-body-wrap2{
    margin-top:10px;
  }
  .c-body-wrap2 .card-text2{
    margin:30px 0;
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

  .cat-list li{
    display:block;
    background: #acacac;
  }

  .dfault-txt{
    margin:120px 0;
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
