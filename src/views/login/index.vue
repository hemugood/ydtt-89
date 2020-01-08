<template>
  <div class="login-contalner">
    <van-nav-bar title="登录" />
    <van-cell-group>
      <van-field v-model="user.mobile" clearable  placeholder="请输入用户手机号" left-icon="contact" />

      <van-field v-model="user.code"  placeholder="请输入验证码" left-icon="contact">
          <van-button
          slot="button"
          size="small"
          type="primary"
          round
          >发送验证码</van-button>
      </van-field>
      <div class="login-btn-wrap">
          <van-button type="info" @click="onLogin">登录</van-button>
      </div>
    </van-cell-group>
  </div>
</template>

<script>
import request from '@/utils/request'
export default {
  name: 'LoginPage',
  components: {},
  props: {},
  data () {
    return {
      user: {
        mobile: '',
        code: ''
      }
    }
  },
  computed: {},
  watch: {},
  created () {},
  methods: {
    async onLogin () {
      const user = this.users
      this.$toast.loading({
        duration: 0,
        message: '登陆中',
        forbidClick: true
      })
      try {
        const res = await request({
          method: 'POST',
          url: '/app/v1_0/authorizations',
          data: user
        })
        console.log(res)
        this.$toast.success('登录成功')
      } catch (err) {
        console.log('登录失败', err)
        this.$toast.fail('登陆失败')
      }
    }
  }
}
</script>

<style lang="less" scoped>
.login-contalner{
    .login-btn-wrap{
        padding:27px 16px;
        .van-button{
            width:100%;
            background: #6db4fb;
        }
    }
    .van-cell {
    height: 45px;
    align-items: center;
  }
}
</style>
