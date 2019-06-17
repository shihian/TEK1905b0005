<template>
  <div class="container">
    <nbaHeader></nbaHeader>
    <main class="main">
      <nuxt class="wrapper" />
    </main>
    <nbaFooter></nbaFooter>
  </div>
</template>

<script>
import nbaHeader from './components/header'
import nbaFooter from './components/footer'

export default {
  components: {
    nbaHeader,
    nbaFooter
  }
}
</script>

<style lang="stylus" scoped>
.container
  display flex
  flex-direction column
  min-height 100vh
  height 100%
.main
  display flex
  flex-flow column nowrap
  // flex 1 0 auto
  flex 1 0
  flex-basis calc(100vh - 300px - 76px)
  // height calc(100vh - 300px - 76px)
  @media $fourInch
    flex-basis calc(100vh - 250px - 76px)
.wrapper
  width 1200px
  margin 0 auto
  .main &
    width 1000px
    @media $smallScreen
      width 900px
    @media $tablet
      width 600px
    @media $mobile
      width 90%
</style>
