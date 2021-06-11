<template>
  <div class="menu-wrapper">
    <transition name="slide">
      <div class="book-menu" :class="{'noboxshadow': childMenuShow || !menushow }" v-show="menushow">
        <div><span class="iconfont icon-xianxingtubiaozhizuomoban-39 icon"></span></div>
        <div><span class="iconfont icon-progress icon"></span></div>
        <div @click="toggleSet(1)"><span class="iconfont icon-brightj2 icon"></span></div>
        <div @click="toggleSet(0)"><span class="icon">A</span></div>
      </div>
    </transition>
    <transition name="slide">
      <div class="setting-box" v-show="childMenuShow">
        <div class="font-setting" v-if="showTag === 0">
          <div class="preview" :style="{fontSize:fontSizeList[0].fontSize+'px'}">A</div>
          <div class="font-select">
             <div class="select-wrapper" v-for="(item,index) in fontSizeList" :key="index">
                <div class="line"></div>
                <div class="point-wrapper" @click="setFontSize(index)">
                  <div class="point" v-show="selectFont ==index">
                    <div class="small-point"></div>
                  </div>
                </div>
                <div class="line"></div>
              </div>
          </div>
        <div class="preview" :style="{fontSize:fontSizeList[fontSizeList.length-1].fontSize+'px'}">A</div>
        </div>
        <div class="themes-setting" v-if="showTag ===1">
          <div class="themes-wrapper" v-for="(item,index) in themeList" :key="index" @click="setTheme(index)">
            <div class="bg--box" :style="{background:item.style.body.background}" :class="{'bg-border':item.style.body.background ==='#fff'}"></div>
            <div class="bg-des" :class="{'default-desc':defaultTheme === index }">{{item.name}}</div>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'bookMenu',
  props: {
    menushow: {
      type: Boolean,
      default: false
    },
    fontSizeList: {
      type: Array,
      default: () => {
        return []
      }
    },
    themeList: {
      type: Array,
      default: () => {
        return []
      }
    },
    defaultTheme: Number
  },
  data () {
    return {
      childMenuShow: false,
      selectFont: 2,
      showTag: 0
    }
  },
  methods: {
    toggleSet (tag) {
      if (this.showTag === tag || !this.childMenuShow) {
        this.childMenuShow = !this.childMenuShow
      }
      this.showTag = tag
    },
    hideChildMenu () {
      this.childMenuShow = false
    },
    setFontSize (index) {
      this.selectFont = index
      this.$emit('setFontSize', this.fontSizeList[index].fontSize)
    },
    setTheme (index) {
      this.$emit('setTheme', index)
    }
  }
}
</script>

<style scoped lang="scss" rel='stylesheet/scss'>
@import './book-menu.scss';
</style>
