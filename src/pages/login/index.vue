<template>
  <div>
    <button open-type="getUserInfo" @getuserinfo="bindGetUserInfo" @click="getUserInfoClick">获取权限</button>
  </div>
</template>

<script>
  export default {
    data () {
      return {}
    },
    mounted () {
      // 函数执行 判断是否已经授权
      this.AlreadyGrant()
    },
    methods: {
      // 函数定义 如果已经授权的情况
      AlreadyGrant: function () {
        // 判断查看是否授权
        wx.getSetting({
          success: function (res) {
            console.log(res)
            // 查看是否授权过
            let grant = res.authSetting['scope.userInfo']
            if (grant) {
              // 如果已经授权过，则直接跳转到首页
              let url = '../index/main'
              wx.navigateTo({url})
            }
          }
        })
      },
      // 函数定义 点击开始授权
      getUserInfoClick () {
        const _this = this
        // 函数执行 开始登录
        _this.wxLogin()
      },
      // 函数定义 开始登录
      wxLogin () {
        const _this = this
        wx.login({
          success (res) {
            // 获取code
            if (res.code) {
              _this.code = res.code
              // 函数执行 开始授权
              _this.wxGetUserInfo(res.code)
            }
          }
        })
      },
      // 函数定义 开始授权
      wxGetUserInfo (code) {
        // 定义请求接口
        var miniProEncrypt = 'https://plus-api.kankan.com' + '/user/login/miniProEncrypt'
        wx.getUserInfo({
          withCredentials: true,
          success (res) {
            var params = {
              terminal: 'MINIPROVJ',
              code: code,
              iv: res.iv
            }
            const newparams = Object.assign(params)
            // 开始执行封装的请求
            wx.request({
              url: miniProEncrypt,
              data: newparams,
              method: 'GET',
              // dataType: JSON,
              header: {
                'content-type': 'application/json' // 默认值
              },
              success: res => {
                console.log(res)
                if (res.data.data) {
                  // 授权成功之后，跳转到首页
                  let url = '../index/main'
                  wx.navigateTo({url})
                } else {
                  console.log('授权失败')
                }
              }
            })
          }
        })
      },
      // 函数定义 判断授权弹窗是否点击了允许
      bindGetUserInfo (e) {
        const _this = this
        // 此处判断是否点击了允许
        if (e.mp.detail.userInfo) {
          _this.getUserInfoClick()
        } else {
          console.log('点击了拒绝')
        }
      }
    }
  }
</script>

<style scoped>

</style>
