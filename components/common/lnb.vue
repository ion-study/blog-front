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

</style>
