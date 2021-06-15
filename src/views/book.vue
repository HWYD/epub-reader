<template>
  <div class="book-wrapper">
    <div id="reader"></div>
    <div class="mask">
      <div class="left" @click="prePage"></div>
      <div class="center" @click="toggleMenu"></div>
      <div class="right" @click="nextPage"></div>
    </div>
    <book-menu ref="bookMenu"
              :menushow="menuShow"
              :fontSizeList = fontSizeList
              :themeList = "themeList"
              :defaultTheme="defaultTheme"
              @setFontSize ="setFontSize"
              @setTheme="setTheme"
              @progressChange="setProgress">
    </book-menu>
  </div>
</template>

<script>
import Epub from 'epubjs'
import BookMenu from '@/components/book-menu'
export default {
  name: 'book',
  components: {
    BookMenu
  },
  data () {
    return {
      book: '',
      rendition: '',
      menuShow: false,
      fontSizeList: [
        { fontSize: 12 },
        { fontSize: 14 },
        { fontSize: 16 },
        { fontSize: 18 },
        { fontSize: 20 },
        { fontSize: 22 },
        { fontSize: 24 }
      ],
      themeList: [
        {
          name: 'default',
          style: {
            body: {
              color: '#000', background: '#fff'
            }
          }
        },
        {
          name: 'eye',
          style: {
            body: {
              color: '#000', background: '#ceeaba'
            }
          }
        },
        {
          name: 'night',
          style: {
            body: {
              color: '#fff', background: '#000'
            }
          }
        },
        {
          name: 'gold',
          style: {
            body: {
              color: '#000', background: 'rgb(241, 236, 226)'
            }
          }
        }
      ],
      defaultTheme: 0,
      navigation: {},
      locations: '',
      bookAvailable: false
    }
  },
  mounted () {
    this.showBook()
  },
  methods: {
    showBook () {
      this.book = new Epub('/山海经.epub')
      this.rendition = this.book.renderTo('reader', { width: window.innerWidth, height: window.innerHeight, method: 'default' })
      this.rendition.display()
      this.themeList.forEach(item => {
        this.rendition.themes.register(item.name, item.style)
      })
      this.book.ready.then(() => {
        this.navigation = this.book.navigation
        return this.book.locations.generate()
      }).then(res => {
        this.locations = this.book.locations
        this.bookAvailable = true
      })
    },
    prePage () {
      this.rendition.prev()
    },
    nextPage () {
      this.rendition.next()
    },
    toggleMenu () {
      this.menuShow = !this.menuShow
      if (!this.menuShow) {
        this.$refs.bookMenu.hideChildMenu()
      }
    },
    setFontSize (fontSize) {
      if (this.rendition.themes) {
        this.rendition.themes.fontSize(fontSize + 'px')
      }
    },
    setTheme (index) {
      this.defaultTheme = index
      this.rendition.themes.select(this.themeList[index].name)
    },
    setProgress (e) {
      const per = e / 100
      const location = per > 0 ? this.locations.cfiFromPercentage(per) : 0
      this.rendition.display(location)
    }
  }
}
</script>

<style scoped lang="scss" rel='stylesheet/scss'>
@import './book.scss';
</style>
