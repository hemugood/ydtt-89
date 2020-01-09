<template>
  <div class="login-contalner">
    <van-nav-bar title="登录" />
    <ValidationObserver ref="form">
      <ValidationProvider name="手机号" rules="required|mobile" v-slot="{errors}" immediate>
        <van-field v-model="user.mobile" clearable placeholder="请输入用户手机号" left-icon="contact" />
        <!-- <span>{{errors[0]}}</span> -->
      </ValidationProvider>
      <ValidationProvider name="验证码" rules="required|code" immediate>
        <van-field v-model="user.code" placeholder="请输入验证码" left-icon="contact">
          <van-count-down
            v-if="isCountDownShow"
            slot="button"
            :time="1000*5"
            format=" ss s"
            @finish="isCountDownShow = false"
          />
          <van-button
            v-else
            slot="button"
            size="small"
            type="primary"
            round
            @click="onSendSmsCode"
          >发送验证码</van-button>
        </van-field>
      </ValidationProvider>
    </ValidationObserver>
    <div class="login-btn-wrap">
      <van-button type="info" @click="onLogin">登录</van-button>
    </div>
  </div>
</template>

<script>
import { login, getSmsCode } from '@/api/user'
import { validate } from 'vee-validate'
export default {
  name: 'LoginPage',
  components: {},
  props: {},
  data () {
    return {
      user: {
        mobile: '',
        code: ''
      },
      isCountDownShow: false
    }
  },
  computed: {},
  watch: {},
  created () {},
  methods: {
    async onLogin () {
      const user = this.user
      // this.$refs.form.validate().then(success => {
      //   if (!success) {
      //   }
      // })
      const success = await this.$refs.form.validate()
      if (!success) {
        console.log('验证失败')
        // console.log(this.$refs.form.errors)
        const errors = this.$refs.form.errors
        for (let key in errors) {
          const item = errors[key]
          if (item[0]) {
            this.$toast(item[0])
            return
          }
        }

        return
      }
      this.$toast.loading({
        duration: 0,
        message: '登陆中...',
        forbidClick: true
      })
      try {
        const { data } = await login(user)
        this.$store.commit('setUser', data.data)
        // console.log(res)
        this.$toast.success('登录成功')
      } catch (err) {
        console.log('登录失败', err)
        this.$toast.fail('登陆失败,手机号或验证码')
      }
    },
    async onSendSmsCode () {
      try {
        const { mobile } = this.user
        const validateResult = await validate(mobile, 'required|mobile', {
          name: '手机号'
        })

        if (!validateResult.valid) {
          this.$toast(validateResult.errors[0])
          return
        }

        const res = await getSmsCode(mobile)
        console.log(res)
        this.isCountDownShow = true
      } catch (err) {
        console.log(err)
        this.$toast('别特么点了，等一会')
      }
    }
  }
}
</script>

<style lang="less" scoped>
.login-contalner {
  .login-btn-wrap {
    padding: 27px 16px;
    .van-button {
      width: 100%;
      background: #6db4fb;
    }
  }
  .van-cell {
    height: 45px;
    align-items: center;
  }
}
</style>
