<template>
  <div id="app">
    <div class="main">
      <div class="header">
        <Search @searchList='handleSearchList' />
      </div>
      <div class="container">
        <div class="content">
          <ul v-for="c  in firstLetterList" :key="c" :ref="c" class="first-letter-ul">
            <h6>{{c}}</h6>
            <li v-for="(item, key) in obj[c]" :key="key" @click="handleShowDialog(item, key)">
              {{item.name}}
            </li>
          </ul>
        </div>
        <div class="indexWrap">
          <div @click="prev">向上</div>
          <div class="indexContent" ref="indexContent">
            <ul>
              <li v-for="c in firstLetterList" :key="c" @click="goTo(c)">{{c}}</li>
            </ul>
          </div>
          <div @click="next">向下</div>
        </div>
      </div>
    </div>
    <Dialog :show.sync="showDialog" :info='info' @switch="handleSwitch" />
  </div>
</template>

<script>
import list from './data/data.json'
import Dialog from './components/Dialog.vue'
import Search from './components/Search.vue'

export default {
  name: 'App',
  components: { Dialog, Search },
  computed: {
    firstLetterList() {
      return Object.keys(this.obj).sort()
    },
  },
  data() {
    return {
      keyword: '',
      list: [],
      obj: {},
      showDialog: false,
      info: {},
    }
  },
  methods: {
    initData(list) {
      const arr = {}
      list.forEach((r) => {
        const key = r.name.slice(0, 1).toUpperCase()
        if (!arr[key]) {
          arr[key] = []
        }
        arr[key].push(r)
      })
      this.obj = arr
    },
    goTo(c) {
      this.$refs[c][0].scrollIntoView()
    },
    handleSearchList(keyword) {
      console.log(12)
      const list = this.list.filter((r) => r.name.indexOf(keyword) > -1)
      this.initData(list)
    },
    handleShowDialog(item, key) {
      this.showDialog = true
      this.info = item
      this.indexKey = key
    },
    handleSwitch(letterIndex, p) {
      const index = this.indexKey + p
      if (this.obj[letterIndex][index]) {
        this.info = this.obj[letterIndex][index]
        this.indexKey = index
      } else {
        const nextArr = this.obj[String.fromCodePoint(letterIndex.charCodeAt() + p)]
        if (!nextArr) {
          alert('已经切换到最后一项')
          return
        }
        const nextIndex = p === 1 ? 0 : nextArr.length - 1
        this.indexKey = nextIndex
        this.info = nextArr[nextIndex]
      }
    },
    next() {
      if (this.page === this.totalPage) {
        return
      }
      this.page++
      this.$refs.indexContent.scrollTop = this.clientHeight * this.page
    },
    prev() {
      if (this.page === 0) {
        return
      }
      this.page--
      this.$refs.indexContent.scrollTop = this.clientHeight * this.page
    },
    initPage() {
      const clientHeight = this.$refs.indexContent.clientHeight
      const scrollHeight = this.$refs.indexContent.scrollHeight
      this.clientHeight = clientHeight
      this.totalPage = clientHeight * Math.floor(scrollHeight / clientHeight)
      this.page = 0
    },
  },
  created() {
    this.list = list
    this.initData(this.list)
  },
  mounted() {
    this.initPage()
  },
}
</script>
<style>
body {
  margin: 0;
}
ul {
  list-style-type: none;
}
</style>
<style scoped>
.main {
  height: 100vh;
  box-sizing: border-box;
  padding: 20px;
  display: flex;
  flex-direction: column;
}
.container {
  display: flex;
  overflow: hidden;
}
.content {
  flex: auto;
  overflow: auto;
}
.indexWrap {
  display: flex;
  flex-direction: column;
  margin: 100px 10px;
}
.indexContent {
  overflow: hidden;
}
.indexContent ul {
}
.first-letter-ul h6 {
  border-bottom: 1px solid #999;
  margin-right: 50px;
}
.first-letter-ul li {
  cursor: pointer;
}
</style>
