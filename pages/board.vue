<template>
  <div>
    <lnb :catList="catList" />
<!--    <pre style="text-align:left;">{{catTree}}</pre>-->
<!--    <pre style="text-align:left;">-->
<!--    {{catList}}-->
<!--    </pre>-->
    <nuxt-child />
  </div>
</template>

<script>
  import lnb from '~/components/common/lnb'
  export default {
    name: "board",
    components: {lnb},
    async asyncData ({ app }) {
      const data = await app.$axios.$get('categories?blogId=1') // 아직 블로그id 관리 X
      data.sort((a,b)=>a.orderNum > b.orderNum)
      return {
        catList: data
      }
    },

    data () {
      return {
        catList: [],
        catTree: []
      }
    }
  }
</script>

<style scoped>

</style>
