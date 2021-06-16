<template>
  <div class="menu-wrapper">
    <transition name="slide">
      <div class="book-menu" :class="{'noboxshadow': childMenuShow || !menushow }" v-show="menushow">
        <div @click="openContent"><span class="iconfont icon-xianxingtubiaozhizuomoban-39 icon"></span></div>
        <div @click="toggleSet(2)"><span class="iconfont icon-progress icon"></span></div>
        <div @click="toggleSet(1)"><span class="iconfont icon-brightj2 icon"></span></div>
        <div @click="toggleSet(0)"><span class="icon">A</span></div>
      </div>
    </transition>
    <transition name="slide">
      <div class="setting-box" v-show="childMenuShow">
        <div class="font-setting" v-if="showTag === 0">
          <div class="preview" :style="{fontSize:fontSizeList[0].fontSize+'px'}">A</div>
          <div class="font-select">
             <div class="select-wrapper" v-for="(item,index) in fontSizeList" :key="index" @click="setFontSize(index)">
                <div class="line"></div>
                <div class="point-wrapper">
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
        <div class="progress-setting" v-if="showTag === 2">
          <input type="range" min ="0" max = "100" step="1" class="progress" @change ="progressChange" @input="progressInput"/>
          <div class="range-tip"><span>{{progress+'%'}}</span></div>
        </div>
      </div>
    </transition>
    <book-content :navigation ="navigation" @goContent="goContent" class="book-content" v-show="contentShow"></book-content>
    <transition name="fade">
      <div class="content-mask" v-show="contentShow" @click="hideContent">
      </div>
    </transition>
  </div>
</template>

<script>
import BookContent from '@/components/content/content'
import { ref } from 'vue'
export default {
  name: 'bookMenu',
  components: {
    BookContent
  },
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
    defaultTheme: Number,
    navigation: {
      type: Object,
      default: () => {
        return {}
      }
    }
  },

  emits: ['setFontSize', 'setTheme', 'progressChange', 'goContent'],

  setup (props, { emit }) {
    const childMenuShow = ref(false)
    const selectFont = ref(2)
    const showTag = ref(0)
    const progress = ref(0)
    const contentShow = ref(false)

    const toggleSet = (tag) => {
      if (showTag.value === tag || !childMenuShow.value) {
        childMenuShow.value = !childMenuShow.value
      }
      showTag.value = tag
    }

    const hideChildMenu = () => {
      childMenuShow.value = false
    }

    const setFontSize = (index) => {
      selectFont.value = index
      emit('setFontSize', props.fontSizeList[index].fontSize)
    }

    const setTheme = (index) => {
      emit('setTheme', index)
    }

    const progressChange = (e) => {
      emit('progressChange', e.target.value)
    }

    const progressInput = (e) => {
      progress.value = e.target.value
    }

    const openContent = () => {
      contentShow.value = true
    }

    const hideContent = () => {
      contentShow.value = false
    }

    const goContent = (href) => {
      emit('goContent', href)
      hideContent()
      hideChildMenu()
    }

    return {
      childMenuShow,
      selectFont,
      showTag,
      progress,
      contentShow,
      toggleSet,
      hideChildMenu,
      setFontSize,
      setTheme,
      progressChange,
      progressInput,
      openContent,
      hideContent,
      goContent
    }
  }
}
</script>

<style scoped lang="scss" rel='stylesheet/scss'>
@import './book-menu.scss';
</style>
