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
    this.category = JSON.parse(`{"gender":"М","athletes":["118081982","122127054","126509124"],"athletesCount":3,"type":"matrix","nodes":{"athletesInfo":[{"userId":"118081982","number":"1"},{"userId":"122127054","number":"2"},{"userId":"126509124","number":"3"}],"laps":[[{"winner":0,"videoLink":"C:\\\\Users\\\\Dell XPS\\\\AppData\\\\Roaming\\\\Electron\\\\Judo\\\\data\\\\media\\\\ira5hAQvJ9pBptHhd","video":"ira5hAQvJ9pBptHhd","score":"2--2","time":"01:23","opponent":2},{"winner":0,"videoLink":"C:\\\\Users\\\\Dell XPS\\\\AppData\\\\Roaming\\\\Electron\\\\Judo\\\\data\\\\media\\\\ira5hAQvJ9pBptHhd","video":"ira5hAQvJ9pBptHhd","score":"2--2","time":"01:23","opponent":1},{"winner":null,"videoLink":null,"video":null,"score":"0-0","time":"00:00","opponent":null}],[{"winner":null,"videoLink":null,"video":null,"score":"0-0","time":"00:00","opponent":3},{"winner":null,"videoLink":null,"video":null,"score":"0-0","time":"00:00","opponent":null},{"winner":null,"videoLink":null,"video":null,"score":"0-0","time":"00:00","opponent":1}],[{"winner":null,"videoLink":null,"video":null,"score":"0-0","time":"00:00","opponent":null},{"winner":null,"videoLink":null,"video":null,"score":"0-0","time":"00:00","opponent":3},{"winner":null,"videoLink":null,"video":null,"score":"0-0","time":"00:00","opponent":2}]],"prizeWinners":[]}}`)
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
