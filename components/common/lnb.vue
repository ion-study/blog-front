<template>
  <div class="menu-category">
    <b-list-group>
      <b-list-group-item button v-for="cat in catList" :key="cat.catId" @click="$router.push(`/board/${cat.catId}`)">{{cat.catName}}</b-list-group-item>
    </b-list-group>
  </div>
</template>

<script>
  export default {
    name: "category",
    props: {
      catList : {
        type: Array
      }
    }
  }
</script>

<style scoped>
  .menu-category{
    width: 100%;
    display: inline-block;
    max-width: 230px;
    float: left;
    margin:30px 0;
  }

  @media (min-width: 768px) and (max-width: 1199px) {
    .menu-category{
      float: inherit;
      max-width:768px;
    }
  }

  @media (min-width: 320px) and (max-width: 767px) {
    .menu-category{
      float: inherit;
      max-width:320px;

    }
  }
</style>
