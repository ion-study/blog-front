<template>
  <div class="menu-category">
    <ul>
      <li v-for="cat in catList" :key="cat.catId">{{cat.catName}}</li>
    </ul>
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

</style>
