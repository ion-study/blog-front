<template>
  <div class="contents-box">
    <div>
      <div class="contents-wrap">
        <div class="aside">
          <div class="profile">
            <p class="image">
              <img src="~assets/img/man.png">
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
                <nuxt-link to="/category/regist" class="button--grey">
                  카테고리 등록
                </nuxt-link>
              </div>
            </div>
          </div>
          <div class="menu-category">
            <h5>카테고리</h5>
            <ul>
              <li>카테고리1</li>
              <li>카테고리2</li>
              <li>카테고리3</li>
              <li>카테고리4</li>

            </ul>
          </div>
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

  .menu-category {
    margin: 20px 0;
    border-right: 1px solid #acacac;
    border-left: 1px solid #acacac;
  }

  .contents {
    width: 80%;
    border: 1px solid #acacac;
    float: right;
  }

  .board-msg p span {
    font-weight: bold;
  }

  .title {
    font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
    display: block;
    font-weight: 300;
    font-size: 100px;
    color: #35495e;
    letter-spacing: 1px;
  }

  .subtitle {
    font-weight: 300;
    font-size: 42px;
    color: #526488;
    word-spacing: 5px;
    padding-bottom: 15px;
  }

  .links {
    padding-top: 15px;

  }
</style>
