<template>
  <div class="search" @click.stop>
    <div class="input">
      <input type="text" v-model="keyword" @focus="showWrapper">
      <button @click="search">搜索</button>
    </div>
    <div class="search-wrapper" v-show="show" @click.stop>
      <ul>
        <li v-for="(item, i) in keywordList" :key="i" @click.stop="setKeyword(item)">{{item.keyword}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Search',
  data() {
    return {
      keyword: '',
      show: false,
      list: [],
    }
  },
  computed: {
    keywordList() {
      return this.list.filter((r) => r.keyword.indexOf(this.keyword) !== -1)
    },
  },
  mounted() {
    const _this = this
    document.addEventListener('click', function () {
      _this.show = false
    })
  },
  methods: {
    setKeyword(item) {
      this.show = false
      this.keyword = item.keyword
    },
    search() {
      const arr = localStorage.searchList ? JSON.parse(localStorage.searchList) : []
      const hasIndex = arr.findIndex((r) => r.keyword === this.keyword)
      if (hasIndex > -1) {
        arr.splice(hasIndex, 1)
      }
      arr.unshift({ date: new Date().getTime(), keyword: this.keyword })
      localStorage.searchList = JSON.stringify(arr)
      this.show = false
      this.$emit('searchList', this.keyword)
    },
    showWrapper() {
      const list = (localStorage.searchList ? JSON.parse(localStorage.searchList) : []).filter(
        (r) => {
          const time = r.date
          return time > new Date().getTime() - 24 * 60 * 60 * 1000
        }
      )
      this.list = list
      localStorage.searchList = JSON.stringify(list)
      this.show = true
    },
  },
}
</script>

<style  scoped>
.search {
  position: relative;
}
.search .input {
  display: flex;
}
.search .input input {
  flex: 1;
}
.search-wrapper {
  position: absolute;
  top: 25px;
  background: #fff;
  border: 1px solid #ccc;
  border-width: 0 1px 1px 1px;
  left: 0;
  right: 0;
}
.search-wrapper ul {
  height: 200px;
  overflow: auto;
}
</style>