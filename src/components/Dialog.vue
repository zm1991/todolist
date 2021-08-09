<template>
  <div class="dialog" @click='hide' v-show="show">
    <div class="container" @click.stop>
      <div class="prev" @click="prev">向上</div>
      <div class="content">
        <div class="info">
          <img :src="info.img" alt="">
          <div>
            <div>
              {{info.name}}
            </div>
            <div><span>{{info.followers}} followers</span> <span>{{info.starts}}</span>starts</div>
          </div>
        </div>
        <div class="email">
          <span>Email:</span>
          {{info.email}}
        </div>
        <div class="bio">
          <span>Bio:</span>
          {{info.bio}}
        </div>
      </div>
      <div class="next" @click="next">向下</div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    info: {
      type: Object,
      default() {
        return {
          name: 1,
        }
      },
    },
    show: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {}
  },
  methods: {
    hide() {
      this.$emit('update:show', false)
    },
    prev() {
      this.$emit('switch', this.info.name.slice(0, 1).toUpperCase(), -1)
    },
    next() {
      this.$emit('switch', this.info.name.slice(0, 1).toUpperCase(), 1)
    },
  },
  components: {},
}
</script>

<style scoped>
.dialog {
  position: fixed;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  background: rgba(255, 255, 255, 0.8);
  padding: 80px 20px;
}
.container {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.content {
  flex: auto;
  border: 1px solid #999;
  border-radius: 8px;
  background: #fff;
  padding: 30px;
}
.prev {
  text-align: center;
  cursor: pointer;
}
.next {
  text-align: center;
  cursor: pointer;
}
.info {
  display: flex;
  align-items: center;
}
.info img {
  margin-right: 100px;
}
</style>
