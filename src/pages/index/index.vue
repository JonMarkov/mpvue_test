<template>
  <div>
    <div v-for="(item, index) in Get_category" :key="index">{{index+1}} {{item.name}}</div>
    <div v-for="(item, index) in Post_category" :key="index">{{index+1}} {{item.name}}</div>
  </div>
</template>

<script>
  import {get, post} from '@/utils/http.js'

  export default {
    data () {
      return {
        Get_category: {},
        Post_category: {}
      }
    },
    mounted () {
      // 函数执行 请求接口--GET
      this.getCategory()
      // 函数执行 请求接口--POST
      this.postCategory()
    },
    methods: {
      // 函数定义 请求接口--GET
      async getCategory () {
        let header = {
          'content-type': 'application/json' // 默认值
        }
        // 封装参数集合
        let params = {}
        const newparams = Object.assign(params)
        // 开始执行封装的请求
        const data = await get('/microvision/getSetListByProductId', newparams, header)
        // 打印出返回的数据
        console.log(data)
      },
      // 函数定义 请求接口--POST
      async postCategory () {
        let header = {
          'content-type': 'application/json' // 默认值
        }
        // 封装参数集合
        let params = {}
        const newparams = Object.assign(params)
        // 开始执行封装的请求
        const data = await post('/microvision/doLike', newparams, header)
        // 打印出返回的数据
        console.log(data)
      }
    }
  }
</script>

<style scoped>

</style>
