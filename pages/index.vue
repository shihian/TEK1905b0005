<template>
  <div class="home">
    <div v-if="gameStatus === 'hasNotStated'" class="start">
      <div class="start-button" @click="gameStart">STATR</div>
    </div>
    <div v-else-if="gameStatus === 'gaming'" class="game">
      <!-- 狀態區 -->
      <div class="status">
        <div class="progress-block">
          <div class="progress-bar">
            <span class="progress-bar-block" :style="progressBarStyle"></span>
          </div>
          <div class="progress-text">
            <span class="progress-text-block">{{ progress }}</span>
          </div>
        </div>
        <div class="second-block">
          <span class="second-text">{{ spend }}</span>
        </div>
      </div>
      <div class="questions">
        <!-- 題目區 -->
        <div class="questions-block">
          <div class="questions-player">
            <!-- <img :src="currentQuestion.playerImg" /> -->
            <img :src="currentImg" />
          </div>
          <div class="button-group">
            <span
              v-for="(opt, index) in currentQuestion.options"
              :key="index"
              class="button button-basic"
              @click="ans(opt.answer)"
              v-text="opt.name"
            ></span>
          </div>
        </div>
      </div>
      <!-- 進度條 -->
      <div class="spend-bar">
        <span class="spend-bar-block" :style="spendBarStyle"></span>
      </div>
      <!-- <button @click="gameFinish">Finish</button> -->
    </div>
    <div v-else class="finish">
      <span class="finish-text">你的NBA金頭腦評分</span>
      <div class="finish-score">
        <span>{{ totalScore }}</span>
      </div>
      <div class="button-group">
        <a class="button-basic fb" href=""><span>分享到 FaceBook</span></a>
        <div class="button-basic again" @click="init">
          <span>再玩一次</span>
        </div>
      </div>
      <form class="form userData" name="userData" method="POST">
        <p>請填入您的 暱稱與 Email 地址，<br />供我們寄送贈品給您：</p>
        <label for="nickname">
          <input
            class="nickname"
            type="txet"
            name="nickname"
            placeholder="暱稱"
          />
        </label>
        <label for="email">
          <input class="email" type="email" name="email" placeholder="email" />
        </label>
        <label for="vpic">
          <input
            class="vpic"
            type="text"
            name="vpic"
            maxlength="4"
            placeholder="驗證碼"
          />
          <a class="change_key" href="javascript:;">
            <img class="keyImage" src="//event.udn.com/event/admin/keyimg" />
          </a>
        </label>
        <a class="button-basic submit" href="javascript:;">確定送出</a>
      </form>
    </div>
    <div class="button-group">
      <span class="button-basic prize" @click="prize">活動贈品</span>
      <span class="button-basic explain" @click="explain">注意事項</span>
      <span class="button-basic leaderboard" @click="leaderboard">排行榜</span>
    </div>
    <div class="button-group ends">
      <span class="button-basic prize" @click="prize">活動贈品</span>
      <span class="button-basic explain" @click="explain">注意事項</span>
      <span class="button-basic leaderboard" @click="leaderboard">排行榜</span>
      <span class="button-basic winning" @click="winning">得獎名單</span>
    </div>
  </div>
</template>

<script>
import Swal from 'sweetalert2'
import axios from 'axios'

const status = {
  hasNotStated: 'hasNotStated',
  gaming: 'gaming',
  end: 'end'
}
// const all = [{id: 123, img: '...a.jpg', name: 'abc'}]
// const q = Array.from({ length: 30 }, function(value, idx) {
//   const id = 203911
//   const img = 'https://dc.udn.com/ian/TEK1905B0005/img/0.jpg'
//   const options = getOptions(id)
//   return { id, img, options }
// })

// function getOptions(id) {
//   const o3 = all.find(star => star.id === id)
//   // o1.answer = true
//   const o2 = all.find(star => star.id !== id)
//   // o1.answer = false
//   const o1 = all.find(star => star.id !== id)
//   // o1.answer = false
//   return [o1, o2, o3]
//     .map(option => {
//       option.answer = option.id === id
//       return option
//     })
//     .sort((a, b) => (Math.random() > 0.5 ? 1 : -1))
// }
// const a = []
// for(let i=0; i<30; i++) {
//   const a
//   a.push(a)
// }
// a.map(function() {

// })
export default {
  components: {},
  data() {
    return {
      gameStatus: status.hasNotStated,
      currentIndex: 0, // 當前
      dataSet: [],
      // dataSet: [
      //   // demo 五筆資料
      //   {
      //     id: 203911,
      //     playerImg: 'https://dc.udn.com/ian/TEK1905B0005/img/0.jpg',
      //     options: [
      //       {
      //         name: 'Jeremy Lamb',
      //         answer: false
      //       },
      //       {
      //         name: 'Jeremy Lin - correct',
      //         answer: true
      //       },
      //       {
      //         name: 'Yuta Watanabe',
      //         answer: false
      //       }
      //     ]
      //   },
      //   {
      //     id: 203912,
      //     playerImg: 'https://dc.udn.com/ian/TEK1905B0005/img/1.jpg',
      //     options: [
      //       {
      //         name: 'LeBron James - correct',
      //         answer: true
      //       },
      //       {
      //         name: 'Kyrie Irving',
      //         answer: false
      //       },
      //       {
      //         name: 'Stephen Curry',
      //         answer: false
      //       }
      //     ]
      //   },
      //   {
      //     id: 203913,
      //     playerImg: 'https://dc.udn.com/ian/TEK1905B0005/img/2.jpg',
      //     options: [
      //       {
      //         name: 'Kyrie Irving',
      //         answer: false
      //       },
      //       {
      //         name: 'Michael Jordan - correct',
      //         answer: true
      //       },
      //       {
      //         name: 'Stephen Curry',
      //         answer: false
      //       }
      //     ]
      //   },
      //   {
      //     id: 203914,
      //     playerImg: 'https://dc.udn.com/ian/TEK1905B0005/img/3.jpg',
      //     options: [
      //       {
      //         name: 'James Harden',
      //         answer: false
      //       },
      //       {
      //         name: 'Kevin Durant',
      //         answer: false
      //       },
      //       {
      //         name: 'Jordan Bell - correct',
      //         answer: true
      //       }
      //     ]
      //   },
      //   {
      //     id: 203915,
      //     playerImg: 'https://dc.udn.com/ian/TEK1905B0005/img/4.jpg',
      //     options: [
      //       {
      //         name: 'James Harden - correct',
      //         answer: true
      //       },
      //       {
      //         name: 'Jordan Bell',
      //         answer: false
      //       },
      //       {
      //         name: 'Michael Jordan',
      //         answer: false
      //       }
      //     ]
      //   }
      // ],
      newQuestions: [], // 建立新陣列資料，舊有資料不動
      score: 0, // 得分
      spend: 60, // 倒數秒數 60秒,
      timer: null,
      progressBarStyle: {
        width: 0
      },
      spendBarStyle: {
        width: 0
      }
    }
  },
  // 計算屬性
  computed: {
    // 當前題數
    currentQuestion() {
      // return this.questions[this.currentIndex]
      return this.newQuestions[this.currentIndex]
    },
    currentImg() {
      const filename = this.currentQuestion.id + '.png'
      // if (!baseURL.endWith('/')) baseURL += '/'
      return process.env.imgBaseUrl + filename
      // /img/West/ATL/162646.jpg
    },
    // 總題數
    totalQuestionsCount() {
      return this.dataSet.length
    },
    progress() {
      // 串接字串寫法一
      // return this.currentIndex + 1 + '/' + this.totalQuestionsCount
      // 串接字串寫法二(優)
      return `${this.currentIndex + 1}/${this.totalQuestionsCount}`
    },
    totalScore() {
      // return `${this.score}/${this.totalQuestionsCount}`
      return `${this.score}`
    },
    questionNo() {
      return this.currentIndex + 1
    }
  },
  watch: {
    spend(newValue, oldValue) {
      if (newValue < 0) {
        this.gameFinish()
      }
    },
    currentIndex() {
      // console.log('新題號', this.questionNo)
      this.progressBarStyle.width = `calc(100% * ${this.questionNo} / ${this.totalQuestionsCount})`
    },
    currentSpend() {
      this.spendBarStyle.width = `calc((${this.currentSpend} / ${this.totalSpend}) * 100%)`
    },
    dataSet() {
      this.suffle()
    }
  },
  created() {
    // console.log(this.dataSet) // 題目
    this.init()
    this.getQuestions()
  },
  mounted() {
    // 建立題目
    // this.qs = this.dataset.map(question => {})
  },
  methods: {
    getQuestions() {
      // this.$axios
      //   .$get('https://event.udn.com/event/playerquiz/show_question.jsp')
      //   .then(response => {
      //     this.dataSet = response.dataSet
      //   })
      axios({
        methods: 'get',
        url: 'https://event.udn.com/event/playerquiz/show_question.jsp'
      }).then(response => {
        const { status, statusText, data } = response
        console.log(status, statusText, data)
        this.dataSet = data.dataSet
      })
    },
    init() {
      this.gameStatus = status.hasNotStated
      this.currentIndex = 0 // 當前題數歸 0
      this.score = 0 // 得分歸 0
      this.spend = 10 // 倒數秒數 60秒
      this.totalSpend = 10 // 總秒數 60秒
      this.currentSpend = 0
      this.progressBarStyle.width = `calc(100% * ${this.questionNo} / ${this.totalQuestionsCount})`
      this.spendBarStyle.width = `calc((${this.currentSpend} / ${this.totalSpend}) * 100%)`
    },
    gameStart() {
      this.gameStatus = status.gaming
      this.timer = this.countDown()
      this.secbar = this.countSpend()
    },
    gameFinish() {
      this.gameStatus = status.end
      clearInterval(this.timer)
      clearInterval(this.secbar)
      console.log('答對幾題:' + this.score)
    },
    ans(answerCorrect) {
      // console.log(typeof answerCorrect)
      // console.log(`答題結果:${answerCorrect}`)
      if (typeof answerCorrect === 'boolean' && answerCorrect) {
        console.log('答對')

        this.score += 1
      } else {
        console.log('答錯')
      }

      if (this.questionNo === this.totalQuestionsCount) {
        this.gameFinish()
      } else {
        this.currentIndex += 1
      }
    },
    suffle() {
      this.newQuestions = this.dataSet
        .slice(0) // 複製"新"陣列
        // this.newQuestions = this.dataSet
        //   .slice(0)
        //   .map(question => {
        //     // 選項順序洗牌
        //     question.options.sort((a, b) => (Math.random() > 0.5 ? 1 : -1))
        //     return question
        //   })
        .sort((a, b) => (Math.random() > 0.5 ? 1 : -1)) // 題目順序洗牌
      console.log(this.newQuestions)
    },
    countDown() {
      return setInterval(() => {
        if (this.spend > -1) {
          // console.log(this.spend--) // 5 4 3 2 1
          // this.spend -= 1 // this.spend = this.spend - 1
          // console.log((this.spend -= 1)) // 4 3 2 1 0
          // console.log(--this.spend) // 5 4 3 2 1
          --this.spend
        }
      }, 1000)
    },
    countSpend() {
      return setInterval(() => {
        if (this.currentSpend > -1) {
          // console.log((this.currentSpend += 1))
          this.currentSpend += 1
          this.spendBarStyle.width = `calc((${this.currentSpend} / ${this.totalSpend}) * 100%)`
          // console.log(this.spendBarStyle.width)
        }
      }, 1000)
    },
    prize() {
      Swal.fire({
        title: '活動贈品',
        html: `<div class="inline_content inline_content_prize">
            <strong>
              <ul>
                <li>當日最高分  贏得NBA公仔</li>
              </ul>
              <img src="http://dc.udn.com/ian/TEK1905B0005/img/prize.jpg">
              <ul>
                <li>答對20題以上  贏得NBA長傘</li>
              </ul>
              <img src="http://dc.udn.com/ian/TEK1905B0005/img/prize2.jpg">
              <ul>
                <li>每日最高分得獎名單將於7/10公布</li>
              </ul>
            </strong>
          </div>`,
        showCloseButton: true,
        confirmButtonText: '關閉'
      })
    },
    explain() {
      Swal.fire({
        title: '注意事項',
        html: `<div class="inline_content">
		        <strong>
              <ul>
                <li>NBA 聯盟相關的人員及球隊的任何成員、活動任何型式的贊助商以及宣傳的相關人員(包括相關的代理商與公關公司)，或者是任何跟上述這些人員相關的家庭或同居成員皆不能參加此活動或者是贏取活動獎項。</li>
                <li>獎項不能隨意轉換或變現，也不可交換或想改變獎項內容，但如果獎項目前因為某種原因無法取得可由贊助商自行決定是否更換，但更換之項目必須為等值商品。得獎者必須要自行支付所有相應產生的稅金以及其他可能之花費。</li>
                <li>所有活動相關辦法，皆以本篇發文公佈為主，獎品項目則依實物為主。</li>
                <li>中獎網友填寫資料之目的係作為確認身分，以便進行活動及獎品領取。聯合線上保證登入資料不洩漏予第三人，亦不進行前述目的範圍以外之利用。未依規定詳填資料（姓名、E-Mail）， 致網友有任何損失者，聯合線上恕不負責。</li>
                <li>本活動得獎資料如有不符合資格或取消者皆不遞補。所有獎項皆不重複得獎(包括 同IP)，如有發現偽造資格或不法得獎者，聯合線上皆有權取消得獎資格。</li>
                <li>參加者於參加本活動同時，即同意接受本活動之活動辦法與注意事項規範，並須遵守聯合線上的服務條款，若發現有使用網頁機器人參與活動違反之規定，聯合線上得取消其參加或得獎資格，並就因此所生之損害，得向參加者請求損害賠償。</li>
                <li>參加者應保證所有填寫或提出之資料均為真實且正確，且未冒用或盜用任何第三人之資料。如有不實 或不正確之情 事，聯合線上得取消參加或得獎資格。如因此致聯合線上無法通知其得獎訊息時，聯合線上不負任何法律責任，且如有致損害於聯合 線上或其他任何第三人，參加者應負一切相關責任。</li>
                <li>得獎者應於聯合線上通知之期限內回覆確認同意領取獎品，並提供聯合線上所要求之完整領獎文件， 逾期視為棄權。</li>
                <li>如有任何因電腦、網路、電話、技術或不可歸責於聯合線上之事由，而使參加者所寄出或登錄之資料 有遲延、遺失、 錯誤、無法辨識或毀損之情況致使參加者無法參加活動時，聯合線上不負任何法律責任，參加者亦不得因此異議。</li>
                <li>如本活動因不可抗力或其他特殊原因致無法舉行時，聯合線上有權決定取消、終止、修改或暫停本活動。</li>
                <li>活動獎項以公佈於本網站上的資料為準，如遇不可抗拒或非可歸責於聯合線上之因素，致無法提供原訂獎項時，聯合線上保留更換其他等值獎項之權利。</li>
                <li>依中華民國稅法規定，獎項金額若超過新台幣 1,000 元，得獎人須依規定填寫並繳交相關中獎收據。得獎者若無法依相關規定配合主辦單位辦理，則視為自動棄權，不具得獎資格。獎項價值超過新台幣 20,000 元者，中獎者依法需自付 10% 稅額 (海外人士或不具中華民國身分者，機會中獎所得稅為20%)。贈品廠商將開立各類所得稅扣繳憑單給中獎者。如果得獎者不願意給付得獎商品之稅額，則視為得獎者自動棄權，不具得獎資格。</li>
                <li>參加者如因參加本活動或因活動獎項而遭受任何損失，聯合線上及相關之母公司、子公司、關係企業、員工、及代理商不負任何責任。一旦得獎者領取獎品後，若有遺失或被竊，聯合線上或贊助廠商等不發給任何證明或補償。</li>
                <li>獎項寄送地區僅限台、澎、金、馬，聯合線上不處理郵寄獎品至海外地區之事宜。本活動之獎品不得轉換、轉讓或折換現金。</li>
                <li>參加者（包含相關繼承者、執行者或管理者）對於獲得的贈品，不可對 NBA 相關單位以及其相關人員（主席、官員、員工、代理商、關係企業、廣告商、公關、贈品贊助商）提出質疑、要求、損壞賠償，或者是做出任何行為或是具有抗爭事實（對於人或者是活動及贈品）的行為，亦不可鼓勵他人和自己去發生對於贈品要求補償的行為。</li>
                <li>其他未盡事宜，悉依主辦單位相關規定。若有任何活動問題，請來信 vaservice@udn.com。</li>
              </ul>
		        </strong>
		      </div>`,
        showCloseButton: true,
        confirmButtonText: '關閉'
      })
    },
    leaderboard() {
      Swal.fire({
        title: '排行榜 RANKING',
        html: `<div class="inline_content inline_content_leaderboard">
            <strong>
              <ul class="table-head">
                <li class="date">日期</li>
                <li class="nickname">暱稱</li>
                <li class="correct">答對題數</li>
              </ul>
              <ul class="table-row list-content">
                <li>
                  <div class="date">2019/06/17</div>
                  <div class="nickname">ian</div>
                  <div class="correct">30</div>
                </li>
                <li>
                  <div class="date">2019/06/17</div>
                  <div class="nickname">ian</div>
                  <div class="correct">30</div>
                </li>
                <li>
                  <div class="date">2019/06/17</div>
                  <div class="nickname">shih</div>
                  <div class="correct">30</div>
                </li>
                <li>
                  <div class="date">2019/06/17</div>
                  <div class="nickname">shihian</div>
                  <div class="correct">30</div>
                </li>
                <li>
                  <div class="date">2019/06/17</div>
                  <div class="nickname">shihyian</div>
                  <div class="correct">30</div>
                </li>
                <li>
                  <div class="date">2019/06/17</div>
                  <div class="nickname">shihshih</div>
                  <div class="correct">30</div>
                </li>
                <li>
                  <div class="date">2019/06/17</div>
                  <div class="nickname">CHU</div>
                  <div class="correct">30</div>
                </li>
              </ul>
            </strong>
          </div>`,
        showCloseButton: true,
        confirmButtonText: '關閉'
      })
    },
    winning() {
      Swal.fire({
        title: '得獎名單',
        html: `<div class="inline_content inline_content_winning">
            <strong>
              <p>【金頭腦獎】NBA公仔</p>
              <ul>
                <li>je***e070207@gmail.com</li>
                <li>ch***un.huang@gmail.com</li>
                <li>wa***ndsee59@hotmail.com</li>
                <li>li***1005@gmail.com</li>
                <li>ch***un.huang@gmail.com</li>
                <li>li***1005@gmail.com</li>
                <li>wa***ndsee59@hotmail.com</li>
                <li>je***e070207@gmail.com</li>
              </ul>
              <p>【金頭腦獎】NBA長傘</p>
              <ul>
                <li>je***e070207@gmail.com</li>
                <li>ch***un.huang@gmail.com</li>
                <li>wa***ndsee59@hotmail.com</li>
                <li>li***1005@gmail.com</li>
                <li>ch***un.huang@gmail.com</li>
                <li>li***1005@gmail.com</li>
                <li>wa***ndsee59@hotmail.com</li>
                <li>ja***012200@yahoo.com.tw</li>
                <li>a0***901057@gmail.com</li>
                <li>li***1005@gmail.com</li>
                <li>dd***41@gmail.com</li>
                <li>gu***m963741852@gmail.com</li>
              </ul>
            </strong>
          </div>`,
        showCloseButton: true,
        confirmButtonText: '關閉'
      })
    }
  }
}
</script>

<style lang="stylus">
.swal2-shown
  padding-right 10px !important

.swal2-height-auto
  padding-right 0 !important

.swal2-popup
  font-size 16px !important
  background-color #f7f7f7 !important
  @media $mobile
    font-size 13px !important
  .swal2-image
    margin-bottom 0
    height inherit
  .title
    margin-top 0
  .swal2-validation-message
    margin-top 1.25em

.swal2-title
  margin .4em 0 .4em

.swal2-close
  top 23px
  right 5px

.swal2title
  font-size 30px
  font-weight bold

.swal2-remark
  font-size 20px
  color #676565
  margin 20px auto

.inline_content
  padding-right 20px
  padding-left 20px
  max-height 60vh
  overflow-y auto
  strong
    display block
    text-align left
    overflow hidden
  h3
    margin 10px 0
    text-align center
  p
    color #ea0000
    text-align center
  ul
    margin 0
    margin-left -20px
    text-align justify
  li
    padding-top 5px
  img
    width 100%
    margin 10px auto 5px

.table
  &-head
    padding 0
    margin-left 0 !important
    font-family $font-primary
    font-weight 600
    background #ffffff
    display flex
    list-style-type none
    li
      margin-right 2px
      text-align center
      color #ffffff
      font-weight 600
      padding 8px
      background-color #1850b8 !important
      &:last-child
        margin-right 0px
  &-row
    padding 0
    margin-left 0 !important
    font-family $font-primary
    background-color #ffffff
    list-style-type none
    li
      display flex
      padding 0
      &:nth-child(even)
        div
          background-color #eeeeee
    div
      display inline-block
      padding 8px
      text-align center
      margin-right 2px
      // background-color #ffffff
      &:last-child
        margin-right 0
.date
  width 120px
  @media $fourInch
    display none !important
.correct
  width 100px
  @media $fourInch
    width 40%
.nickname
  width calc(100% - 220px)
  @media $fourInch
    width 60%
</style>

<style lang="stylus" scoped>
.start
  &-button
    display block
    cursor pointer
    margin 0 auto
    width 210px
    height 210px
    line-height 210px
    text-align center
    font-size 30px
    font-weight bold
    color #fcd703
    background-image url('~@/assets/img/start-bg.jpg')
    background-repeat no-repeat
    margin-top 50px

.questions
  text-align center
  &-player
    margin-bottom 20px
    img
      display block
      max-width 410px
      width 100%
      height auto
      margin 0 auto

.status
  display flex
  justify-content space-between
  margin 50px auto 30px

.progress
  &-block
    display flex
    align-content space-between
    width calc(100% - 250px)
  &-bar
    position relative
    display block
    width calc(100% - 90px)
    height 50px
    background-color #d5d5d5
    &-block
      position absolute
      top 0
      left 0
      display block
      width 0
      height 100%
      transition .5s
      // transition .1s width linear
      background-color #1d4289
  &-text
    font-size 26px
    color #666666
    width 90px
    text-align center
    line-height 50px

.second
  &-block
    margin-left 10px
    width 240px
    font-size 26px
    height 50px
    background-color #ffffff
    border #999999 solid 4px
    display flex
    justify-content center
    align-items center
  &-text
    &:after
      display inline-block
      margin-left 3px
      font-size 16px
      content '秒'

.spend
  &-bar
    position relative
    margin 30px auto 0
    width 100%
    height 5px
    background-color #d5d5d5
    &-block
      position absolute
      top 0
      left 0
      display block
      width 0
      height 100%
      transition 1s width linear
      background-color #9db1d7

.finish
  margin-top 50px
  text-align center
  &-text
    font-size 30px
    font-weight bold
    line-height 1
  &-score
    display flex
    align-items center
    justify-content center
    margin 10px auto 30px
    padding 30px 0
    min-width: 250px
    max-width 300px
    font-size 50px
    font-weight bold
    color #bb132a
    background-color #ffffff
    outline #999999 solid 4px

.form
  max-width 300px
  margin 0 auto
  p
    font-size 18px
    margin 30px auto 10px
  input
    margin 0 auto 10px
    padding 15px 20px
    max-width 300px
    width 100%
    height 60px
    font-size 16px
    border 4px solid #999
    background-color #ffffff
  [for="vpic"]
    display block
    position relative
  .change_key
    position absolute
    right 4px
    top 10px

.button
  &-group
    display flex
    justify-content center
    margin 50px auto
    @media $mobile
      display block
    .questions &
      margin 0 auto
    .finish &
      margin 20px auto
      display block
  &-basic
    cursor pointer
    display block
    margin 0 10px
    // max-width 300px
    width calc((100% - 20px)/3)
    font-size 25px
    font-weight bold
    color $white
    text-align center
    line-height 1.2
    padding 10px 0
    background-color #e4e4e4
    text-decoration none
    &:last-child
      @media $mobile
        margin-bottom 0
    @media $mobile
      width 100%
      margin 0 auto 10px
    .questions &
      font-size 20px
      color #666666
      // min-width 320px
      max-width inherit
      min-height 120px
      display flex
      justify-content center
      align-items center
    .finish &
      display block
    &.active
      background-color #ffffff
      border 4px solid #1d4289
    &:hover
      opacity 0.85
      text-decoration none
      .questions &
        border 4px solid #999999
    &.winning
      background-color #e74b5e
    &.prize
      background-color #e74b5e
    &.explain
      background-color #9f9c9c
    &.leaderboard
      background-color #1d4289
    &.fb
      &:before
        content '\45'
        font-family "fontello"
        font-style normal
        font-weight normal
        display inline-block
        color #ffffff
        margin-right 5px
      max-width 300px
      width 100%
      background-color #3e5a99
      margin 0 auto 10px
    &.again
      &:before
        content '\67'
        font-family "fontello"
        font-style normal
        font-weight normal
        display inline-block
        color #ffffff
        margin-right 5px
      max-width 300px
      width 100%
      background-color #55a3c9
      margin 0 auto
    &.submit
      max-width 300px
      width 100%
      margin 10px auto 0
      background-color #b8152a
</style>
