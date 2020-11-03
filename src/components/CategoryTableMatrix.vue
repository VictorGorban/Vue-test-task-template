<template>
  <div style="position: relative;">
    <table class="table table-striped table-bordered category-matrix">
      <thead>
      <tr>
        <th v-for="(lap, lapIndex) in laps"
            :class="{'current-lap': lapIndex==currentLapIndex,'next-lap': lapIndex==nextLapIndex}"
        >Круг
          {{ lapIndex + 1 }}
        </th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(info, infoIndex) in category.athletes">
        <td v-for="(lap, lapIndex) in laps" class="lap-cell"
            :class="{'point-on-hover': !isNoValue(lap[infoIndex].opponent)}"
            @click="(!isNoValue(lap[infoIndex].opponent)) ? handleCellClick(infoIndex, lap, lapIndex) : null">
          <div v-if="(!isNoValue(lap[infoIndex].opponent))">
            <div class="opponent-number">{{ lap[infoIndex].opponent }}</div>
            <div class="score">{{ lap[infoIndex].score }}</div>
            <div class="time">{{ lap[infoIndex].time }}</div>
          </div>

          <div v-if="isNoValue(lap[infoIndex].opponent)" class="free-cell">
          </div>
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>

import _ from 'lodash'

export default {
  name: 'ContestTableMatrix',
  props: ['category'],
  data() {
    return {
      laps: [],
    }
  },
  watch: {
    category(newValue) {
      let self = this
      if (newValue.type != 'matrix') return
      self.makeMatrix()
    }
  },
  created() {
    let self = this

  },
  mounted() {
    let self = this

    self.makeMatrix()
  },
  methods: {
    handleCellClick(cellIndex, lap, lapIndex) {
      let self = this
      let originalLap = self.category.nodes.laps[lapIndex]
      let originalCell = lap[cellIndex]
      console.log('selectCell of lap', originalCell, originalLap)
    }
    ,

    isNoValue(value) {
      let self = this
      return value === null || value === undefined
    },
    /**
     * Заполняет данные для корректной отрисовки матрицы
     * @returns {Promise<void>}
     */
    async makeMatrix() { // здесь мы заполняем данные, на основе которых рендерится таблица
      let self = this
      try {
        let cat = self.category

        self.laps = _.cloneDeep(cat.nodes.laps)

      } catch (e) {
        console.error(e)
      }
    }
    ,

  }
}
</script>

<style scoped>
td.lap-cell {
  position: relative;
  min-width: 65px;
  width: 65px;
  height: 60px;
  color: black
}

td.lap-cell:hover {
  background: rgba(42, 63, 84, 0.25)
}

th.info-cell:hover {
  background: rgba(42, 63, 84, 0.25)
}

td .opponent-number {
  position: absolute;
  left: 2px;
  top: 10px;
  font-size: 30px;
}

td .free-cell {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  background: #8080809e
}

td .time {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  text-align: center;
  font-size: 10px;
}

td .score {
  position: absolute;
  right: 5px;
  bottom: 5px;
  font-size: 10px;
}

th.current-lap, td.current-lap {
  background: rgba(38, 185, 154, 0.5);
  color: black;
}

th.next-lap, td.next-lap {
  background: rgba(217, 83, 79, 0.5);
  color: black;
}

th.header-error {
  color: red;
  border: 1px solid red;
}

</style>
