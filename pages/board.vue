<template>
  <div>
    <category :catList="catList" />
<!--    <pre style="text-align:left;">-->
<!--    {{catList}}-->
<!--    </pre>-->
    <nuxt-child />
  </div>
</template>

<script>
  import category from '~/components/common/category'
  export default {
    name: "board",
    components: {category},
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

<style scoped>

</style>
