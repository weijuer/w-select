<template>
  <table class="table table-default table-bordered table-hover">
    <thead class="table-header">
      <tr>
        <th v-for="key in columns" @click="sortBy(key)" :key="key" :class="{ active: sortKey == key }">
          {{ key | capitalize }}
          <span class="arrow" :class="sortOrders[key] > 0 ? 'asc' : 'dsc'"> </span>
        </th>
      </tr>
    </thead>
    <tbody class="table-body">
      <tr v-for="entry in filteredHeroes" :key="entry.id">
        <td v-for="key in columns" :key="key">
          {{ entry[key] }}
        </td>
      </tr>
    </tbody>
    <tfoot class="table-footer">
      <tr>
        <td :colspan="columns.length">
          <div class="table-footer-bar">
            <div class="table-info">
              共 1 条
            </div>
            <div class="table-pagination">
              <ul>
                <li>
                  <a class="btn icon-btn" title="首页" href="javascript:;">
                    <i class="icon first"></i>
                  </a>
                </li>
                <li>
                  <a class="btn icon-btn" title="上一页" href="javascript:;">
                    <i class="icon prev"></i>
                  </a>
                </li>
                <li>1/1</li>
                <li>
                  <a class="btn icon-btn" title="下一页" href="javascript:;">
                    <i class="icon next"></i>
                  </a>
                </li>
                <li>
                  <a class="btn icon-btn" title="尾页" href="javascript:;">
                    <i class="icon last"></i>
                  </a>
                </li>
              </ul>
            </div>
          </div>
        </td>
      </tr>
    </tfoot>
  </table>
</template>

<script>
export default {
  name: 'simple-table',
  props: {
    heroes: Array,
    columns: Array,
    filterKey: String
  },
  data: function() {
    var sortOrders = {}
    this.columns.forEach(function(key) {
      sortOrders[key] = 1
    })
    return {
      sortKey: '',
      sortOrders: sortOrders
    }
  },
  computed: {
    filteredHeroes: function() {
      var sortKey = this.sortKey
      var filterKey = this.filterKey && this.filterKey.toLowerCase()
      var order = this.sortOrders[sortKey] || 1
      var heroes = this.heroes
      if (filterKey) {
        heroes = heroes.filter(function(row) {
          return Object.keys(row).some(function(key) {
            return (
              String(row[key])
                .toLowerCase()
                .indexOf(filterKey) > -1
            )
          })
        })
      }
      if (sortKey) {
        heroes = heroes.slice().sort(function(a, b) {
          a = a[sortKey]
          b = b[sortKey]
          return (a === b ? 0 : a > b ? 1 : -1) * order
        })
      }
      return heroes
    }
  },
  filters: {
    capitalize: function(str) {
      return str.charAt(0).toUpperCase() + str.slice(1)
    }
  },
  methods: {
    sortBy: function(key) {
      this.sortKey = key
      this.sortOrders[key] = this.sortOrders[key] * -1
    }
  }
}
</script>

<style lang="stylus" scoped>
.table {
  width: 100%
  max-width: 100%
  margin-bottom: 20px
  border-collapse: collapse
  border-spacing: 0

  &.table-bordered {
    border: 1px solid #dadada
  }

  > tbody > tr:nth-child(odd) {
    background: #f6f6f6
  }

  > tbody > tr:nth-child(even) {
    background: #fff
  }
}

.table,
.table > thead > tr > th,
.table > thead > tr > td,
.table > tbody > tr > th,
.table > tbody > tr > td,
.table > tfoot > tr > th,
.table > tfoot > tr > td {
  table-layout: fixed
  line-height: 35px
  padding: 0 10px
}

.table > thead > tr > th,
.table > thead > tr > td,
.table > tbody > tr > th,
.table > tbody > tr > td,
.table > tfoot > tr > th,
.table > tfoot > tr > td {
  overflow: hidden
  white-space: nowrap
  vertical-align: middle
  text-overflow: ellipsis
}

.table > tbody > tr > td:last-child {
  text-overflow: initial
}

.table > tfoot > tr,
.table > tfoot > tr > th,
.table > thead > tr > th {
  font-size: 15px
  background: #eee
}

.table-bordered,
.table-bordered > tbody > tr,
.table-bordered > tbody > tr > td,
.table-bordered > tbody > tr > th,
.table-bordered > tfoot > tr > td,
.table-bordered > tfoot > tr > th,
.table-bordered > thead > tr > td,
.table-bordered > thead > tr > th {
  text-align: center
  border: 0
  border-top: 1px solid #dadada
}

.table-bordered > tbody > tr.row-span,
.table-bordered > tbody > tr.row-span > td,
.table-bordered > tbody > tr.row-span > th {
  border-top: 1px dashed #55afda
}

.table > thead > tr > th {
  border-bottom: 0
}

.table > tfoot > tr > th {
  text-align: justify
}

.table-hover > tbody > tr:hover {
  background: #eee
}

.table-scrollable > .table-head-fixed {
  background: #eee
}

.table.table-head-fixed tbody > tr,
.table-head-fixed thead > tr,
.table-head-fixed thead,
.table-head-fixed tfoot > tr,
.table-head-fixed tfoot {
  display: table
  width: 100%
  table-layout: fixed
}

.table-head-fixed tbody {
  display: block
  height: 360px
  overflow: auto
  background: #fff
}

.table-head-fixed tbody:after {
  content: ""
  display: flex
  justify-content: center
  align-items: center
}

.table-head-fixed tbody > tr {
  line-height: 35px
  height: 35px
}

.table-head-fixed tfoot,
.table-head-fixed thead {
  width: calc(100% - 17px)
}

.table-head-fixed.table-bordered > thead > tr > th {
  border-top: 0 !important
}

/* table-footer-bar */
.table-footer-bar {
  display: flex
  justify-content: space-between

  .table-info {
    height: 35px
    line-height: 35px
    text-indent: 16px

    .pager-size {
      width: auto
      height: 24px
      padding: 0 2px
    }
  }

  .table-pagination {
    margin-right: 8px

    ul {
      margin: 0
      padding: 0
      list-style: none
      display: flex

      > li {
        flex: 1
        display: flex
        justify-content: center
        align-items: center

        > a {
          margin: 0
          padding: 0 8px
          color: #666
          text-decoration: none
        }
      }
    }
  }
}

.btn {
  display: inline-flex
  justify-content: center
  align-items: center
}

.icon {
  width: 20px
  height: 20px
  display: inline-flex
  justify-content: center
  align-items: center
  font-size: 22px
  font-style: normal

  &.first {
    position: relative

    &::before {
      display: inline-block
      content: "\25C4"
      transform: scale(0.6, 1.2) translateX(8px)
    }

    &::after {
      display: inline-block
      content: "\25C4"
      transform: scale(0.6, 1.2) translateX(-10px)
    }
  }

  &.prev {
    &::before {
      display: inline-block
      content: "\25C4"
    }
  }

  &.next {
    &::before {
      display: inline-block
      content: "\25BA"
    }
  }

  &.last {
    &::before {
      display: inline-block
      content: "\25BA"
      transform: scale(0.6, 1.2) translateX(8px)
    }

    &::after {
      display: inline-block
      content: "\25BA"
      transform: scale(0.6, 1.2) translateX(-10px)
    }
  }
}
</style>
