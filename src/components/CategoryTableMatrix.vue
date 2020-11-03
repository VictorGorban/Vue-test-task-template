<template>
  <div style="position: relative;">
    <table class="table table-striped table-bordered category-matrix">
      <thead>
      <tr>
        <th></th>
        <th v-for="(lap, lapIndex) in laps"
            :class="{'current-lap': lapIndex==currentLapIndex,'next-lap': lapIndex==nextLapIndex}"
        >Круг
          {{ lapIndex + 1 }}
        </th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(info, infoIndex) in infos">
        <th class="point-on-hover info-cell" :class="{'header-error': twoEqualUserId(info.userId)}"
            @click="handleInfoClick(info, infoIndex)">{{ info.number }}.
          {{ getUserFIOShort(info.user) + infoSuffix(info) }}
        </th>
        <td v-for="(lap, lapIndex) in laps" class="lap-cell"
            :class="{'point-on-hover': (!isNoValue(lap[infoIndex].opponent)), 'current-lap': lapIndex==currentLapIndex,'next-lap': lapIndex==nextLapIndex}"
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

import moment from 'moment'
import _ from 'lodash'

export default {
  name: 'ContestTableMatrix',
  props: ['category'],
  data () {
    return {
      matrixCategory: {},
      currentLapIndex: 0,
      nextLapIndex: 1,
      infos: [],
      results: [],
      laps: [],
    }
  },
  watch: {
    category (newValue) {
      let self = this
      if(newValue.type!='matrix') return
      self.makeMatrix()
    }
  },
  created () {
    let self = this

  },
  mounted () {
    let self = this

    self.makeMatrix()
  },
  methods: {
    infoSuffix (info) {
      if (info.disqualified) {
        return '[ДИСКВ]'
      }
      if (info.notShownUp) {
        return '[Н/Я]'
      }

      return ''
    },

    twoEqualUserId (userId) {
      let self = this
      let filtered = self.category.nodes.athletesInfo.filter(info => info.userId == userId)
      if (filtered.length > 1) {
        return true
      }
      return false
    }
    ,
    handleInfoClick (info, infoIndex) {
      let self = this
      let originalInfo = self.category.nodes.athletesInfo[infoIndex]
      console.log('selectInfo', originalInfo)
    }
    ,
    handleCellClick (cellIndex, lap, lapIndex) {
      let self = this
      let originalLap = self.category.nodes.laps[lapIndex]
      let originalCell = lap[cellIndex]
      console.log('selectCell of lap', originalCell, originalLap)
    }
    ,

    /**
     * Получение Фамилия И.О. для участника user
     * @param user
     * @returns {string}
     */
    getUserFIOShort (user) {
      let self = this
      if (!user) return ''
      let fio = user.name
      if (!fio) return ''
      let parts = fio.split(' ').filter(p => p != '')
      let F = parts[0].substring(0, 10)
      if (parts[0].length - 10 < 2) F = parts[0]
      if (F.length < parts[0].length) {
        F += '..'
      }
      let I = parts[1]
      let O = parts[2]
      let result = F
      if (I) result += ` ${I[0]}.`
      if (O) result += ` ${O[0]}.`

      return result
    }
    ,
    isNoValue (value) {
      let self = this
      return value === null || value === undefined
    },
    getCurrentLapIndex (laps) {
      let self = this;
      for (let [lapIndex, lap] of laps.entries()) {
        for (let cell of lap) {
          if (self.isNoValue(cell.opponent)) { // если это клетка СВ, то просто идем дальше
            continue
          }
          if (self.isNoValue(cell.winner)) // если в этой клетке не поставлен winner, то возвращаем
          {
            return lapIndex
          }
        }
      }
      return undefined // если дошли до сюда, значит currentLapIndex - просто нет
    }
    ,
    /**
     * Заполняет данные для корректной отрисовки матрицы
     * @returns {Promise<void>}
     */
    async makeMatrix () { // здесь мы заполняем данные, на основе которых рендерится таблица
      let self = this
      try {
        let cat = self.category
        let newInfos = []
        for (let info of cat.nodes.athletesInfo) {
          let newInfo = _.cloneDeep(info)
          newInfo.user = window.users.find(user=>user._id == info.userId)
          newInfos.push(newInfo)
        }
        self.infos = newInfos

        self.laps = _.cloneDeep(cat.nodes.laps)

        self.currentLapIndex = self.getCurrentLapIndex(self.laps)
        self.nextLapIndex = self.currentLapIndex + 1
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
