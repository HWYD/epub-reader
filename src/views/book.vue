<template>
  <div class="book-wrapper">
    <div id="reader" ></div>
    <div class="mask" ref="maskRef">
      <div class="left" @click="prePage"></div>
      <div class="center" @click="toggleMenu"></div>
      <div class="right" @click="nextPage"></div>
    </div>
    <book-menu ref="bookMenuRef"
              :menushow="menuShow"
              :fontSizeList = fontSizeList
              :themeList = "themeList"
              :defaultTheme="defaultTheme"
              :navigation="navigation"
              @setFontSize ="setFontSize"
              @setTheme="setTheme"
              @progressChange="setProgress"
              @goContent="goContent">
    </book-menu>
  </div>
</template>

<script>
import Epub from 'epubjs'
import BookMenu from '@/components/menu/book-menu'
import { ref, reactive, onMounted } from 'vue'

const setVal = () => {
  const fontSizeList = reactive([
    { fontSize: 12 },
    { fontSize: 14 },
    { fontSize: 16 },
    { fontSize: 18 },
    { fontSize: 20 },
    { fontSize: 22 },
    { fontSize: 24 }
  ])
  const themeList = reactive([
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
  ])
  return { fontSizeList, themeList }
}

export default {
  name: 'book',
  components: {
    BookMenu
  },
  setup (props, context) {
    onMounted(() => {
      showBook()
      gestureSwitch()
    })

    const { fontSizeList, themeList } = setVal()
    const book = ref({})
    const rendition = ref({})
    const menuShow = ref(false)
    const defaultTheme = ref(0)
    const navigation = ref({})
    const locations = ref({})
    const bookAvailable = ref(false)
    const maskRef = ref(null)
    const bookMenuRef = ref(null)

    const showBook = () => {
      const readerWidth = window.innerWidth > 799 ? '790px' : window.innerWidth
      book.value = new Epub('/resource/book/山海经.epub')
      rendition.value = book.value.renderTo('reader', { width: readerWidth, height: window.innerHeight, method: 'default' })
      rendition.value.display()
      themeList.forEach(item => {
        rendition.value.themes.register(item.name, item.style)
      })
      book.value.ready.then(() => {
        navigation.value = book.value.navigation
        return book.value.locations.generate()
      }).then(res => {
        locations.value = book.value.locations
        bookAvailable.value = true
      })
    }

    const prePage = () => {
      rendition.value.prev()
    }

    const nextPage = () => {
      rendition.value.next()
    }

    const toggleMenu = () => {
      menuShow.value = !menuShow.value
      if (!menuShow.value) {
        bookMenuRef.value.hideChildMenu()
      }
    }

    const setFontSize = (fontSize) => {
      if (rendition.value.themes) {
        rendition.value.themes.fontSize(fontSize + 'px')
      }
    }

    const setTheme = (index) => {
      defaultTheme.value = index
      rendition.value.themes.select(themeList[index].name)
    }

    const setProgress = (e) => {
      const per = e / 100
      const location = per > 0 ? locations.value.cfiFromPercentage(per) : 0
      rendition.value.display(location)
    }

    const goContent = (href) => {
      rendition.value.display(href)
      menuShow.value = false
    }

    // 添加手势滑动进行翻页
    const gestureSwitch = () => {
      let touchStartX, startTime
      maskRef.value.addEventListener('touchstart', e => {
        touchStartX = e.changedTouches[0].clientX
        startTime = e.timeStamp
      })
      maskRef.value.addEventListener('touchend', e => {
        const offsetX = e.changedTouches[0].clientX - touchStartX
        const durTime = e.timeStamp - startTime
        if (offsetX > 40 && durTime < 500) {
          prePage()
        }
        if (offsetX < -40 && durTime < 500) {
          nextPage()
        }
        e.stopPropagation()
      })
    }

    return {
      book,
      rendition,
      menuShow,
      fontSizeList,
      themeList,
      defaultTheme,
      navigation,
      locations,
      bookAvailable,
      maskRef,
      showBook,
      bookMenuRef,
      prePage,
      nextPage,
      toggleMenu,
      setFontSize,
      setTheme,
      setProgress,
      goContent,
      gestureSwitch
    }
  }
}
</script>

<style scoped lang="scss" rel='stylesheet/scss'>
@import './book.scss';
</style>
