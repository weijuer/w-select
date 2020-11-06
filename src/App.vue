<template>
  <div id="app">
    <form class="demo-form hidden" action>
      <fieldset>
        <legend>单选</legend>
        <w-select search :options="options" v-model="selected"></w-select>
      </fieldset>

      <fieldset>
        <legend>多选</legend>
        <w-select search multiple :options="options" v-model="selecteds"></w-select>
      </fieldset>
    </form>

    <simple-table :heroes="gridData" :columns="gridColumns" :filter-key="searchQuery"></simple-table>
  </div>
</template>

<script>
import Select from './components/Select.vue'
import SimpleTable from './components/SimpleTable.vue'

export default {
  name: 'App',
  components: {
    [Select.name]: Select,
    SimpleTable
  },
  data() {
    return {
      options: [],
      selected: '',
      selecteds: [],
      searchQuery: '',
      gridColumns: ['name', 'power'],
      gridData: [
        { name: 'Chuck Norris', power: Infinity },
        { name: 'Bruce Lee', power: 9000 },
        { name: 'Jackie Chan', power: 7000 },
        { name: 'Jet Li', power: 8000 }
      ]
    }
  },
  created() {
    this.init()
  },
  methods: {
    init() {
      // 初始化数据
      this.options = this.createOptions(10)
    },
    createOptions(max) {
      // 根据max配置生成自定义个数
      return new Array(max).fill(0).map((option, index) => {
        return {
          text: 'Select-' + (index + 1),
          key: 'key-' + (index + 1)
        }
      })
    }
  }
}
</script>

<style lang="stylus">
html,
body {
  margin: 0
  padding: 0
}

#app {
  font-family: -apple-system, SF UI Text, Arial, PingFang SC, Hiragino Sans GB, Microsoft YaHei, WenQuanYi Micro Hei, sans-serif, SimHei, SimSun
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  text-align: center
  color: #2c3e50
  margin-top: 60px
}

.hidden {
  display: none
}

.demo-form {
  margin: 0 auto
  max-width: 960px

  fieldset {
    margin-bottom: 1em
    border: 1px solid #aaa
  }
}
</style>
