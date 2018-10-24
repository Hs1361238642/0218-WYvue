<template>
  <section class="loginContainer">
    <div class="loginInner">
      <div class="login_header">
        <div data-v-6c60b3a4="" class="logoWrap">
          <img data-v-6c60b3a4="" src="//yanxuan.nosdn.127.net/bd139d2c42205f749cd4ab78fa3d6c60.png" alt="">
        </div>
        <div class="login_header_title">
          <a href="javascript:;" class="on" v-show="!loginWay">短信登录</a>
          <a href="javascript:;" class="on" v-show="loginWay">账号登录</a>
        </div>
      </div>
      <div class="login_content">
        <form @submit.prevent="login">
          <div class="on" v-show="!loginWay">
            <section class="login_message">
              <input type="text" maxlength="11" placeholder="手机号" v-model="phone">
            </section>
            <section class="login_verification">
              <input type="text" maxlength="8" placeholder="验证码" v-model="code">
            </section>
            <section class="login_hint">
              温馨提示：未注册网易严选帐号的手机号，登录时将自动注册，且代表已同意
              <a href="javascript:;">《用户服务协议》</a>
            </section>
          </div>
          <div class="on" v-show="loginWay">
            <section>
              <section class="login_message">
                <input type="text" maxlength="11" placeholder="手机/邮箱/用户名" v-model="name">
              </section>
              <section class="login_verification">
                <input type="text" maxlength="8" placeholder="密码" v-if="showPwd" v-model="pwd">
                <input type="password" maxlength="8" placeholder="密码" v-else v-model="pwd">
                <div class="switch_button" :class="showPwd?'on':'off'" @click="showPwd=!showPwd">
                  <div class="switch_circle" :class="{right: showPwd}"></div>
                  <span class="switch_text">{{showPwd ? 'abc' : '...'}}</span>
                </div>
              </section>
              <section class="login_message">
                <input type="text" maxlength="11" placeholder="验证码" v-model="captcha">
                <img class="get_verification" src="../../common/img/captcha.svg" alt="captcha"
                      ref="captcha" style="width: 300px">
              </section>
            </section>
          </div>
          <button class="login_submit">登录</button>
        </form>
        <a href="javascript:;" class="about_us">关于我们</a>
      </div>
      <a href="javascript:" class="go_back" @click="$router.back()">
        <i class="iconfont icon-jiantou2"></i>
      </a>
    </div>
    <AlertTip :alertText="alertText" v-show="alertShow" @closeTip="closeTip"/>
  </section>
</template>

<script>
  import {mapState} from 'vuex'
  //import {accountLogin, reqCaptcha ,mobileCode, phoneLogin} from '../../api'
  import AlertTip from '../../components/AlertTip/AlertTip'
    export default {
      data () {
        return {
          //loginWay: false, // true代表短信登陆, false代表密码
          computeTime: 0, // 计时的时间
          showPwd: false, // 是否显示密码
          phone: '', // 手机号
          code:'', // 短信验证码
          name: '', // 用户名
          pwd: '', // 密码
          captcha: '', // 图形验证码
          alertText: '', // 提示文本
          alertShow: false, // 是否显示警告框
        }
      },

      computed: {
        ...mapState(['loginWay']),
        rightPhone () {
          return /^1\d{10}$/.test(this.phone)
        }
      },

      methods: {
        showAlert(alertText) {
          this.alertShow = true
          this.alertText = alertText
        },
        // 登陆
       login () {
          let result
          // 前台表单验证
          if(!this.loginWay) {  // 短信登陆
            const {code} = this
            if(!this.rightPhone) {
              // 手机号不正确
              this.showAlert('手机号不正确')
              return
            } else if(!/^\d{6}$/.test(code)) {
              // 验证必须是6位数字
              this.showAlert('验证必须是6位数字')
              return
            }
          }
          else {// 密码登陆
            if(!this.name) {
              // 用户名必须指定
              this.showAlert('用户名必须指定')
              return
            } else if(!this.pwd) {
              // 密码必须指定
              this.showAlert('密码必须指定')
              return
            } else if(!this.captcha) {
              // 验证码必须指定
              this.showAlert('验证码必须指定')
              return
            }
          }
         this.showAlert('登陆成功,即将跳转到首页')
         setTimeout(() => {
           this.$router.replace('/home')
           this.closeTip ()
         },1000)

        },
        // 关闭警告框
        closeTip () {
          this.alertShow = false
          this.alertText = ''
        },
      },
      components: {
        AlertTip
      }
    }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixins.styl";
  input:-webkit-autofill{
    -webkit-box-shadow: 0 0 0 100px white inset;
    -webkit-text-fill-color: #666;
  }
  input:-webkit-autofill:focus {
    -webkit-box-shadow: 0 0 0 100px white inset;
    -webkit-text-fill-color: #666;
  }
  .logoWrap
    padding-top 1.13333rem
    padding-bottom .13333rem
    text-align center
    img
      width 3.57333rem
      height 1.2rem
  .loginContainer
    width 100%
    height 100%
    background #FFF
    .loginInner
      padding-top: 60px;
      width: 80%;
      margin: 0 auto;
      .login_header
        font-size 40px
        font-weight bold
        color #333
        text-align center
      .login_header_title
        padding-top 40px
        text-align center
        > a
           height .53333rem
           color #7F7F7F
           margin-lefy .06667rem
           font-size .4rem
           &:first-child
             margin-right 40px
           &.on
             color #b4282d
             font-weight 700
             border-bottom 2px solid #b4282d
      .login_content
        margin-top 30px
        > form
          > div
            display none
            &.on
              display block
            input
              width 100%
              height 100%
              padding-left 10px
              box-sizing border-box
              border 1px solid #999
              font-size 0.4rem
              outline 0
              border-radius 4px
              &:focus
                border 1px solid #b4282d
            .login_message
              margin-bottom .42667rem
              border-radius 2px
              display block
              width 100%
              height 1.25333rem
              line-height 1.25333rem
              overflow hidden
              font-size .37333rem
              color #fff
              margin-top 27px
              text-align center
              vertical-align middle
              .get_verification
                position absolute
                top 49%
                right 60px
                border 0
                color #ccc
                font-size .4rem
                transform translateY(-50%)
                &img
                  width 100px
                &input
                  margin-top 50px
                  background #ffffff!important
                &.right_phone
                  color black
            .login_verification
              position relative
              margin-top 16px
              height  1.25333rem
              font-size 14px
              background #fff
              .switch_button
                border 1px solid #ddd
                position absolute
                top 50%
                right 10px
                font-size 12px
                boeder-radius 20px
                transition background-color .3s, border-color .3s
                padding 0 6px
                width 70px
                height 31px
                line-hight 16px
                color #fff
                transform translateY(-50%)
                &.off
                  background #fff
                  .switch_text
                    float right
                    color #ddd
                &.on
                  background #b4282d
                > .switch_circle
                  position absolute
                  top -1px
                  left -1px
                  width 30px
                  height 30px
                  border 1px solid #ddd
                  border-radius 50%
                  background #fff
                  box-shadow 0 2px 4px 0 rgba(0, 0 ,0, .1)
                  transition transform .3s
                  &.right
                    transform translateX(55px)
                > .switch_text
                  font-size 0.34rem
            .login_hint
              margin-top 12px
              color #999
              font-size .3rem
              line-height .55rem
              > a
                color #b4282d
          .login_submit
            display block
            width 100%
            font-size 0.4rem
            line-height 42px
            margin-top 30px
            border-radius 4px
            text-align center
            height 1.25333rem
            background #b4282d
            color #fff
            border 0
        .about_us
          display block
          font-size 0.4rem
          text-align center
          color #999
          margin-top 20px
      .go_back
        position absolute
        top 5px
        left 5px
        width 30px
        height 30px
        > .iconfont
          font-size 20px
          color black
</style>
