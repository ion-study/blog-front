<template>
  <div class="menu-category">
    <b-list-group>
      <template v-for="cat in catTree">
        <b-list-group-item button :key="cat.catId" @click="clickCat(cat)"><template v-if="setup">[{{cat.orderNum}}] </template>{{cat.catName}}</b-list-group-item>
        <b-list-group-item v-for="subCat in cat.child" button :key="subCat.catId" @click="clickCat(subCat)">┗ <template v-if="setup">[{{subCat.orderNum}}]</template>{{subCat.catName}}</b-list-group-item>
      </template>
    </b-list-group>
  </div>
</template>

<script>
  export default {
    name: "category",
    props: {
      catList : {
        type: Array
      },
      setup : {
        type: Boolean,
        default: false
      }
    },
    data () {
      return {
        catTree: []
      }
    },
    computed: {
      orderCat () {
        return this.catList.slice().sort((a,b) => a.orderNum - b.orderNum)
      }
    },
    watch: {
      catList () {
        this.makeTree()
      }
    },
    created () {
      this.makeTree()
    },
    methods: {
      clickCat (cat) {
        if (!this.setup) this.$router.push(`/board/${cat.catId}`)
        else {
          this.$emit('sel-cat', cat)
        }
      },
      makeTree () {
        this.catTree = this.orderCat.filter((cat)=>cat.depth==0)
        this.subCats = this.orderCat.filter((cat)=>cat.depth>0)
        this.catTree.forEach((upper)=>{
          let child = []
          this.subCats.forEach((subCat)=>{
            if (upper.catId === subCat.parentCatId) child.push(subCat)
          })
          if (child.length>0) upper.child = child
        })
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
    text-align:left;
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
