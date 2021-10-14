<template>
<div class="wrapper">
  <div class="page" @click="changePage(page)"
  v-for="page in totalPage"
  :key="page"
  :class="{'activeP' : page === curPage}"
  >
  {{page}}</div>
</div>
</template>

<script>
export default {
    name: 'my-pagination',
    props: ['totalPage', 'curPage'],
    data(){
      return {
        active: false
      }
    },
    methods: {
      changePage(page){
        console.log(page)
        this.$emit("updatePage", page);
      }
    }
}
</script>

<style scoped>
.wrapper{
  display: flex;
  width: 90%;
  margin-top: 20px;
  justify-content: space-between;
}
  .page{
    cursor: pointer;
    border: 1px solid black;
    width: 30px;
    text-align: center;
    margin-top: 3px;
  }

  .activeP{
    border-color: red;
    color: red;
  }
</style>