<template>
  <div class="w-select" :class="{'active': display}">
    <div class="selected">
      <input
        class="w-select-control"
        :title="checkedText"
        v-model="checkedText"
        @click="onToggle"
        readonly
        type="text"
        placeholder="请选择"
      />
    </div>
    <div class="w-dropmenu">
      <div v-if="search" class="w-dropmenu-search">
        <input class="w-select-control" type="text" v-model="filterKey" placeholder="请输入关键字" />
      </div>
      <transition-group class="w-dropmenu-list" name="dropdown" tag="ul">
        <slot>
          <li
            @click="onSelect(option)"
            v-for="(option, index) of searchOptions"
            :key="'option-' + index"
            class="w-dropmenu-item"
            :class="isChecked(option)"
          >
            <span>{{ option.text }}</span>
            <i v-if="icon" class="iconfont" :class="[ option.key ]"></i>
          </li>
          <li v-if="searchOptions.length === 0" class="w-dropmenu-item">
            <span>暂无数据</span>
          </li>
        </slot>
      </transition-group>
    </div>
  </div>
</template>

<script>
export default {
  name: 'w-select',
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    options: Array, // 选项源数据 [{key: x, text: x}]
    checked: [String, Array], // 勾选值，字符串或数组，默认字符串
    multiple: { type: Boolean, default: false }, // 是否多选
    search: { type: Boolean, default: false }, // 是否开启搜索
    icon: Boolean // 列表是否有图标
  },
  data: function() {
    return {
      display: false,
      filterKey: ''
    }
  },
  computed: {
    checkedText: function() {
      var checked = this.checked
      var textArr = []
      this.options.map(function(option) {
        if (checked.indexOf(option.key) > -1) {
          textArr.push(option.text)
        }
      })
      return textArr.join()
    },
    searchOptions: function() {
      var filterKey = this.filterKey
      var data = this.options
      if (filterKey) {
        data = data.filter(function(option) {
          return (
            String(option['text'])
              .toLowerCase()
              .indexOf(filterKey) > -1
          )
        })
      }
      return data
    }
  },
  mounted() {
    this.init()
  },
  methods: {
    init: function() {
      document.addEventListener('click', this.onBlur, false)
      // 取消注册
      this.$once('hook:beforeDestroy', function() {
        document.removeEventListener('click', this.onBlur, false)
      })
    },
    onToggle: function() {
      this.display = !this.display
    },
    onBlur: function(e) {
      if (!this.$el.contains(e.target)) {
        this.display = false
      }
    },
    isChecked: function(option) {
      var active = false
      // 多选
      if (this.multiple) {
        active = this.checked.indexOf(option.key) > -1
      } else {
        active = this.checked === option.key
      }
      return {
        active: active,
        disabled: option.disabled
      }
    },
    onSelect: function(option) {
      var key = option.key
      // 多选
      if (!option.disabled) {
        if (this.multiple) {
          // 拷贝一份
          var arr = this.checked.slice()
          var index = arr.indexOf(key)
          // 已勾选
          if (index > -1) {
            arr.splice(index, 1)
          } else {
            arr.push(key)
          }
          this.$emit('change', arr)
        } else {
          this.display = false
          this.$emit('change', key)
        }
      }
    }
  }
}
</script>

<style lang="stylus">
.w-select
  position: relative

  .selected
    display: flex
    align-items: center
    border: 1px solid #ccc

    &:after
      content: "\25BC"
      display: inline-block
      font-size: 14px
      color: #5e656b
      position: absolute
      right: 6px

    .w-select-control
      width: 100%
      padding: 4px 6px
      min-height: 20px
      line-height: 20px
      font-size: 14px
      text-overflow: ellipsis
      color: #5e656b
      border: 0
      outline: none

  .w-dropmenu
    width: calc(100% - 2px)
    position: absolute
    top: 100%
    left: 0
    z-index: -1
    border: 1px solid #ccc
    border-top: 0
    background-color: #fff
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175)
    visibility: hidden

    .w-dropmenu-search
      padding: 12px 24px
      display: flex
      justify-content: center
      font-size: 14px
      border-bottom: 1px solid #eee

      .w-select-control
        width: 100%
        line-height: 20px
        border: 1px solid #ccc
        outline: none

    .w-dropmenu-list
      margin: 0
      padding: 0
      min-width: 120px
      max-height: 200px
      overflow: auto
      list-style: none
      font-size: 14px
      text-align: left

      &::-webkit-scrollbar
        width: 4px
        height: 4px

      &::-webkit-scrollbar-thumb
        border-radius: 4px
        background: transparent
        transition: all 0.3s linear

      &::-webkit-scrollbar-track
        background: transparent
        border-radius: 4px

      &:hover
        &::-webkit-scrollbar-thumb
          background: #c1c1c1

      > li
        display: flex
        justify-content: space-between
        align-items: center
        padding: 4px 24px
        position: relative
        clear: both
        font-weight: 400
        line-height: 1.42857143
        color: #333
        white-space: nowrap
        text-decoration: none
        user-select: none

        &:hover
          background: #3498db
          color: #fff

        &.active:before
          content: "\2714"
          display: inline-block
          position: absolute
          left: 8px

  &.active
    .selected:after
      content: "\25B2"

    .w-dropmenu
      z-index: 2
      visibility: visible
</style>
