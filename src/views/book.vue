<template>
  <div class="book-wrapper">
    <div id="reader"></div>
    <div class="mask">
      <div class="left" @click="prePage"></div>
      <div class="center" @click="toggleMenu"></div>
      <div class="right" @click="nextPage"></div>
    </div>
    <book-menu ref="bookMenu" :menushow="menuShow"></book-menu>
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
      menuShow: false
    }
  },
  mounted () {
    this.showBook()
  },
  methods: {
    showBook () {
      this.book = new Epub('/山海经.epub')
      console.log(this.book)
      this.rendition = this.book.renderTo('reader', { width: window.innerWidth, height: window.innerHeight })
      this.rendition.display()
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
    }
  }
}
</script>

<style scoped lang="scss" rel='stylesheet/scss'>
@import './book.scss';
</style>
