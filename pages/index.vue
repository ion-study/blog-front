<template>
  <div class="contents-box">
    <div>
      <div class="contents-wrap">
        <div class="aside">
          <div class="profile">
            <p class="image">
              <img src="~assets/img/man.png" alt="user-icon"/>
            </p>
            <p class="profile-txt">
              <span>홍길동</span>의 블로그 입니다.
            </p>
            <div class="btn-wrap">
              <div class="links">
                <nuxt-link to="/board/regist" class="button--green">
                  게시글 등록
                </nuxt-link>
              </div>
              <div class="links pb30">
                <nuxt-link to="/category" class="button--grey">
                  카테고리 관리
                </nuxt-link>
              </div>
            </div>
          </div>
          <lnb :catList="catList" />
        </div>
        <div class="contents">

          <div class="board-list">
            <div class="board-msg">
              <p class="pt30"><span class="c-green">아직 작성된 글이 없습니다.</span> <br/>문득 스치는 생각이나 기분, 일기 등 다양한 이야기로
                나만의 공간을 채워보세요!</p>
              <div class="links pb30">
                <nuxt-link to="/board/regist" class="button--green">
                  게시글 등록
                </nuxt-link>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import lnb from '~/components/common/lnb'
  export default {
    components: {lnb},
    async asyncData ({ app }) {
      const data = await app.$axios.$get('categories?blogId=1') // 아직 블로그id 관리 X
      // console.log(data)
      return {
        catList: data
      }
    },
    data () {
      return {
        catList: []
      }
    }
  }
</script>


<style>
  .contents-wrap {
    width: 100%;
    overflow: hidden;
  }

  .aside {
    width: 18%;
    float: left;
  }

  .aside .profile {
    width: 200px;
    display: block;
  }

  .aside .profile .image {
    text-align: center;
    border-radius: 10px;
    border: 1px solid #acacac;
    min-height: 185px;
    line-height: 185px;
  }

  .profile-txt {
    min-height: 20px;
    text-align: left;
    padding: 0 10px;
  }

  .profile-txt span {
    color: #3b8070;
  }

  .btn-wrap {
    display: inline-block;
  }

  .btn-wrap .links:nth-child(2) {
    margin-left: -15px;
  }
  .btn-wrap .links {
    margin-left: 0;
  }
  .contents {
    width: 80%;
    border: 1px solid #acacac;
    float: right;
  }
  .board-msg{
    height: 450px;
    padding: 150px 0;
  }
  .board-msg p span {
    font-weight: bold;
  }
  .links {
    padding-top: 15px;
  }

  @media (min-width: 768px) and (max-width: 1199px) {
    .contents-box {
      max-width: 768px;
      margin: 0 auto;
    }
    .aside{
      width:100%;
      max-width: 768px;
    }
    .aside .profile {
      width:100%;
    }
    .aside .profile .btn-wrap .links{
      float:left;
      margin:0 20px;
    }
    .aside .menu-category {
      width:100%;
      max-width:768px;
    }
    .contents {
      width: 100%;
      max-width:768px;
      padding:0 10px;
      border: 1px solid #acacac;
      font-size:15px;
    }
  }
  @media (min-width:768px) and (max-width:1199px) {

  }
  @media (min-width: 320px) and (max-width: 767px) {
    .contents-box {
      max-width: 320px;
      margin: 0 auto;
    }
    .aside{
      width:100%;
    }
    .aside .profile {
      max-width: 320px;
      margin:0 auto;
    }
    .board-msg p {
      padding-top:0;
    }
    .contents {
      width: 100%;
      max-width:320px;
      padding:0 10px;
      border: 1px solid #acacac;
      font-size:15px;
    }

  }
</style>
