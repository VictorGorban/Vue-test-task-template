<template>
  <div id="app">
    <CategoryTableMatrix
      :category="category"
    />
    <router-view/>
  </div>
</template>

<script>
import CategoryTableMatrix from '@/components/CategoryTableMatrix'

export default {
  components: {
    CategoryTableMatrix,
  },
  data(){
    return {
      category: null,
      currentUser: null,
      currentCell: null,
      currentOriginalLapIndex: null,
      currentCellIndex: null
    }
  },
  created(){
    this.category = JSON.parse(`{"athletes":["118081982","122127054","126509124"],"type":"matrix","nodes":{"laps":[[{"videoLink":null,"score":"2-2","time":"01:23","opponent":2},{"videoLink":null,"score":"2--2","time":"01:23","opponent":1},{"videoLink":null,"score":"0-0","time":"00:00","opponent":null}],[{"videoLink":null,"score":"0-0","time":"00:00","opponent":3},{"videoLink":null,"score":"0-0","time":"00:00","opponent":null},{"videoLink":null,"score":"0-0","time":"00:00","opponent":1}],[{"videoLink":null,"score":"0-0","time":"00:00","opponent":null},{"videoLink":null,"score":"0-0","time":"00:00","opponent":3},{"videoLink":null,"score":"0-0","time":"00:00","opponent":2}]]}}`)
  },
  methods:{},
}
</script>

<style lang="scss">

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

</style>
