<template>
  <!-- // header -->
  <header class="header">
    <div class="wrapper">
      <div class="logo-group">
        <a class="logo udnLogo" href="//udn.com/news/index" target="_blank">
          <span>udnLogo</span>
        </a>
        <a class="logo nbaLogo" href="//nba.udn.com/" target="_blank">
          <span>nbaLogo</span>
        </a>
      </div>
      <div class="text text-group">
        <img src="~@/assets/img/nba-title.png" alt="NBA金頭腦" />
        <div class="text-words">
          <strong>60</strong><span>秒</span>內盡可能猜球員名字
        </div>
        <div class="text-words"><strong>30/30</strong>你能對幾題？</div>
      </div>
    </div>
  </header>
</template>

<script>
export default {}
</script>

<style lang="stylus" scoped>
.header
  background-image url('~@/assets/img/header-bg.jpg')
  background-position top center
  display flex
  flex-flow column nowrap
  flex 0 0 300px
  @media $fourInch
    flex 0 0 250px

.wrapper
  width 1200px
  margin 0 auto
  position relative
  height 300px
  display flex
  justify-content center
  align-items center
  @media $smallScreen
    width 1000px
  @media $tablet
    width 730px
  @media $mobile
    width 90%
  @media $fourInch
    height 250px

.logo
  &-group
    position absolute
    display flex
    left 30px
    top 30px
    @media $tablet
      left 0
      top 20px
    @media $mobile
      top 30px
    .logo
      display block
      margin-right 20px
      background-size cover
      text-indent -9999px
      &:last-child
        margin-right 0
    .udnLogo
      width 200px
      height 50px
      background-image url('~@/assets/img/udnLogo.png')
      background-repeat no-repeat
      @media $smallScreen
        width 160px
        height 40px
      @media $mobile
        width 140px
        height 35px
    .nbaLogo
      width 79px
      height 50px
      background-image url('~@/assets/img/nbaLogo.png')
      background-repeat no-repeat
      @media $smallScreen
        width 63px
        height 40px
      @media $mobile
        width 55px
        height 35px
    img
      display block
      width 100%

.text
  &-group
    color $white
    font-size 28px
    line-height 1
    text-align center
    @media $tablet
      padding-top 40px
    @media $mobile
      font-size 1.6rem
    @media $fourInch
      padding-top 50px
      font-size 1.4rem
  &-words
    margin-bottom 20px
    @media $tablet
      margin-bottom 10px
    &:last-child
      margin-bottom 0
  img
    @media $fourInch
      width 100%
  strong
    color $yellow
    font-size 50px
    @media $mobile
      font-size 2.4rem
    @media $fourInch
      font-size 2rem
  span
    color $yellow
    font-size 28px
    font-weight bold
    @media $mobile
      font-size 1.6rem
    @media $fourInch
      font-size 1.4rem
</style>
