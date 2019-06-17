<template>
  <!-- footer -->
  <footer class="footer">
    <div class="footer-copyright center">
      <b>聯合線上公司 著作權所有</b> &copy; udn.com All Rights Reserved.
    </div>
  </footer>
</template>

<style lang="stylus" scoped>
.footer
  color $white
  background-color #3e3f3f
  text-align center
  display flex
  flex-flow column nowrap
  flex 0 0 76px
  justify-content center
  align-items center
  &-copyright
    display block
    font-size 15px
    @media $mobile
      font-size 14px
    b
      @media $mobile
        display block
</style>
