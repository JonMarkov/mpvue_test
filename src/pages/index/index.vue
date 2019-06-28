<template>
  <div>
    <div v-for="(item, index) in category" :key="index">{{index+1}} {{item.name}}</div>
  </div>
</template>

<script>
  import {get} from '@/utils/http.js'
  export default {
    data () {
      return {
        category: {}
      }
    },
    mounted () {
      // 函数执行 请求接口
      this.getCategory()
    },
    methods: {
      // 函数定义 请求接口
      async getCategory () {
        // 封装参数集合
        let params = {}
        const newparams = Object.assign(params)
        // 开始执行封装的请求
        const data = await get('/shop/goods/category/all', newparams)
        // 把返回的数据渲染到数据data中
        this.category = data.data
        // 打印出返回的数据
        console.log(data)
      }
    }
  }
</script>

<style scoped>

</style>
