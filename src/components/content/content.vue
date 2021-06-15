<template>
  <transition name="slideH">
    <div class="content-wrapper">
      <div v-for="item in navigation.toc" :key="item">
        <div class="content-txt" @click="goContent(item.href)">{{item.label}}</div>
        <div class="line"></div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'content',
  props: {
    navigation: {
      type: Object,
      default: () => { return {} }
    }
  },
  methods: {
    goContent (href) {
      this.$emit('goContent', href)
    }
  }
}
</script>

<style scoped lang="scss" rel='stylesheet/scss'>
.content-wrapper{
  position: absolute;
  top: 0;
  left: 0;
  width: 80%;
  height: 100%;
  padding: 10px 0;
  font-size: 14px;
  background:#fff;
  overflow-y: scroll;
  .content-txt{
   width: 100%;
   height: 50px;
   padding-top: 30px;
   padding-left: 10px;
  }
  .line{
    width: 100%;
    height: 1px;
    background: rgba(0,0,0,.1);
  }
}

</style>
