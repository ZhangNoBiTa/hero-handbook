<template>
  <div class = "hero-box" ref = 'heroBox'>
    <h3 class = 'hero-title'>英雄分类</h3>
    <div class = "container-typeOf">
      <div
        :class = 'typeItem.className'
        @click = 'toheroType(typeItem.className)'
        v-for = 'typeItem in heroTypes'
        :key = 'typeItem.typeName'>
        <span class = 'hero-type-title'>{{typeItem.typeName}}</span>
      </div>
    </div>
    <h3 class = 'hebdomad-hero-title'>周免英雄</h3>
    <div class="loading-container" v-show ="loadingShow">
      <loading></loading>
    </div>
    <div class="tab" ref="tab" v-show="!loadingShow">
      <ul class="tab_content" ref="tabWrapper">
        <li
          class="tab_item"
          v-for="item in TopHeroList"
          ref="tabitem"
          :key="item.title">
          <img :src="heroImg(item.imgid)" width="60px" v-bind:title="item.title">
          <p class = 'hero-name'>{{item.name}}</p>
        </li>
      </ul>
    </div>
    <h3 class = 'noob-strategy-title'>新手攻略</h3>
    <div class = 'noob-strategy' ref = 'noobStrategy'>
      <a class='nood-title'>电竞专业术语宝典</a>
    </div>
  </div>
</template>

<script>
import {CreateTopHeroList} from 'common/js/herolist'
import Scroll from 'base/scroll/scroll'
import BScroll from 'better-scroll'
import Loading from 'base/loading/loading'

const BACKGAUGE_WIDTH = 30
const TOPHERO_WIDTH = 80

export default{
  data () {
    return {
      heroTypes: [
        {typeName: '坦克', className: 'tank box'},
        {typeName: '战士', className: 'warrior box'},
        {typeName: '刺客', className: 'assassin box'},
        {typeName: '法师', className: 'mage box'},
        {typeName: '射手', className: 'shooter box'},
        {typeName: '辅助', className: 'aid box'}
      ],
      hebdomadList: [],
      TopHeroList: [],
      loadingShow: true
    }
  },
  props: {
    heroList: {
      type: Array
    }
  },
  created () {
    this.eventPassthrough = 'vertical'
    this._getHeroList()
  },
  methods: {
    _getHeroList () {
      this.hebdomadList = this.heroList
      this.TopHeroList = CreateTopHeroList(this.hebdomadList)
      let TabWidth = 0
      setTimeout(() => {
        for (var i = 0; i < this.TopHeroList.length; i++) {
          TabWidth += TOPHERO_WIDTH
        }
        this.$refs.tabWrapper.style.width = TabWidth + 'px'
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.tab, {
            startX: 0,
            click: true,
            scrollX: true,
            scrollY: false,
            eventPassthrough: 'vertical'
          })
          this.loadingShow = false
        } else {
          this.scroll.refresh()
        }
      }, 2000)
    },
    heroImg (imgid) {
      return `http://game.gtimg.cn/images/yxzj/img201606/heroimg/${imgid}/${imgid}.jpg`
    },
    toheroType (typeName) {
      typeName = typeName.split(' ')[0]
      this.$router.push({
        path: `/hero/${typeName}`
      })
    }
  },
  mounted () {
    let ClientWidth = document.body.clientWidth - BACKGAUGE_WIDTH
    let boxs = document.getElementsByClassName('box')
    for (let i = 0; i < boxs.length; i++) {
      let cNames = boxs[i].className
      let cName = cNames.split(' ')
      document.querySelectorAll('.' + cName[0])[0].style.width = ClientWidth / 3 + 'px'
    }
    this.$refs.tab.style.width = ClientWidth + BACKGAUGE_WIDTH - 10 + 'px'
    this.$refs.noobStrategy.style.width = ClientWidth + BACKGAUGE_WIDTH - 30 + 'px'
  },
  components: {
    Scroll,
    Loading
  }
}
</script>

<style lang = 'stylus' rel = 'sheetstyle/stylus' scoped>
@import '~common/stylus/variable.styl'
@import '~common/stylus/mixin.styl'

.hero-box
  position:relative
  background: #ffffff
  top:20px
  left:0
  z-index:1
.hero-title
  position: absolute
  left:20px
  font-weight:600
  color:$color-dialog-background
  font-size:$font-size-medium-x
.container-typeOf
  width: 100%
  position: absolute
  top:30px
  left:0
  .box
    height:110px
    float: left
    background-size:cover
    margin:5px 5px
    text-align:center
    position: relative
    .hero-type-title
      position: absolute
      bottom:15px
      left:15px
      font-size:$font-size-hero-box-text
      color:$color-hero-box-text
  .tank
    bg-images("ctank")
  .warrior
    bg-images("cwarrior")
  .assassin
    bg-images("cassassin")
  .mage
    bg-images("cmage")
  .shooter
    bg-images("carcher")
  .aid
    bg-images("csupport")
.hebdomad-hero-title
  position: absolute
  left: 20px
  top: 300px
  font-weight:600
  color:$color-dialog-background
  font-size:$font-size-medium-x
.noob-strategy-title
  position: absolute
  left:20px
  top: 470px
  font-weight:600
  color:$color-dialog-background
  font-size:$font-size-medium-x
.noob-strategy
  bg-images('jargon')
  background-size:cover
  -webkit-border-radius: 10px
  -moz-border-radius: 10px
  border-radius: 10px
  position: absolute
  top: 510px
  left: 15px
  right: 15px
  height:90px
  text-align: center
  .nood-title
    position: relative
    top:35px
    color: $color-btn-nood
    letter-spacing: 1.5px
    font-size:20px
.loading-container
  position: absolute
  width: 100%
  top:370px
.tab
  overflow: hidden
  padding: 5px
  position: absolute
  top: 340px
  .tab_content
    line-height: 2rem
    display: flex
    .tab_item
      flex: 0 0 60px
      text-align: center
      img
        border-radius: 50%
        margin:0 10px
      .hero-name
        font-size:13px
        color:$color-hero-name
        font-weight:600

</style>
