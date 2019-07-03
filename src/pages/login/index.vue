<template>
  <div>
    <div v-if="canIUse">
      <div class='header'>
        <img src="../../../static/images/logo@3x.png"/>
      </div>
      <div class='content'>
        <div class='title_sq'>申请获取以下权限</div>
        <p>获得你的公开信息(昵称，头像等)</p></div>
      <button open-type="getUserInfo" @getuserinfo="bindGetUserInfo" @click="getUserInfoClick" class="bottom">获取权限
      </button>
    </div>
    <div v-else>请升级微信版本</div>

  </div>
</template>

<script>
  export default {
    data () {
      return {
        // 判断小程序的API，回调，参数，组件等是否在当前版本可用。
        canIUse: wx.canIUse('button.open-type.getUserInfo')
      }
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
  .header {
    margin: 45px 0 45px 25px;
    border-bottom: 1px solid #ccc;
    text-align: center;
    width: 325px;
    height: 150px;
    line-height: 225px;
  }

  .header image {
    width: 100px;
    height: 100px;
    border-radius: 20px
  }

  .content {
    margin-left: 25px;
    margin-bottom: 45px;
  }

  .content text {
    display: block;
    color: #9d9d9d;
    margin-top: 0px;
    font-size: 13px
  }

  .bottom {
    background: #4AC268;
    border-radius: 10px;
    margin: 35px 25px;
    font-size: 13px;
    font-weight: bold;
    padding: 5px;
    color: #ffffff;
  }

  .title_sq {
    font-size: 20px
  }
</style>
