<template>
  <div class="menu-wrapper">
    <transition name="slide">
      <div class="book-menu" :class="{'noboxshadow': childMenuShow || !menushow }" v-show="menushow">
        <div><span class="iconfont icon-xianxingtubiaozhizuomoban-39 icon"></span></div>
        <div><span class="iconfont icon-progress icon"></span></div>
        <div><span class="iconfont icon-brightj2 icon"></span></div>
        <div @click="setFont"><span class="icon">A</span></div>
      </div>
    </transition>
    <transition name="slide">
      <div class="setting-box" v-show="childMenuShow">
        <div class="font-setting">
          <div class="preview" :style="{fontSize:fontSizeList[0].fontSize+'px'}">A</div>
          <div class="font-select">
             <div class="select-wrapper" v-for="(item,index) in fontSizeList" :key="index">
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
    }
  },
  data () {
    return {
      childMenuShow: false,
      fontSizeList: [
        { fontSize: 12 },
        { fontSize: 14 },
        { fontSize: 16 },
        { fontSize: 18 },
        { fontSize: 20 },
        { fontSize: 22 },
        { fontSize: 24 }
      ],
      selectFont: 2
    }
  },
  methods: {
    setFont () {
      this.childMenuShow = !this.childMenuShow
    },
    hideChildMenu () {
      this.childMenuShow = false
    }
  }
}
</script>

<style scoped lang="scss" rel='stylesheet/scss'>
@import '@/assets/style/mixin.scss';
.menu-wrapper{
  .book-menu{
    z-index: 20;
    position: absolute;
    left: 0;
    bottom:0;
    display: flex;
    width: 100%;
    height: 50px;
    box-shadow: 0 -5px 5px rgba(0,0,0,.15);
    background: white;
    >div{
    flex: 1;
    @include flex-center;
    }
    &.noboxshadow{
      box-shadow: none;
    }
  }
  .setting-box{
    z-index: 19;
    position: absolute;
    left: 0;
    bottom:50px;
    width: 100%;
    height: 60px;
    box-shadow: 0 -5px 5px rgba(0,0,0,.15);
    background: white;
    .font-setting{
      display: flex;
      width: 100%;
      height: 100%;
      .preview{
       flex: 0 0 10%;
       @include flex-center;
      }
      .font-select{
        flex: 1;
        display: flex;
        .select-wrapper{
          flex: 1;
          display: flex;
          align-items: center;
          &:first-child{
            .line{
              &:first-child{
                border-top: none;
              }
            }
          }
          &:last-child{
            .line{
              &:last-child{
                border-top: none;
              }
            }
          }
          .line{
            flex: 1;
            height: 0;
            border-top: 1px solid #ccc;
          }
          .point-wrapper{
            position: relative;
            flex: 0 0 0;
            width: 0;
            height: 7px;
            border-left: 1px solid #ccc;
          }
          .point{
            position: absolute;
            top: -8px;
            left: -8px;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: white;
            border: 1px solid #ccc;
            box-shadow: 0 4px 4px rbga(0,0,0,.15);
            @include flex-center;
            .small-point{
              width: 5px;
              height: 5px;
              background: black;
              border-radius: 50%;
            }
          }
        }

      }
    }
  }
}
</style>
