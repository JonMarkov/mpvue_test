<template>
  <div>

  </div>
</template>

<script>
  import {get} from '@/utils/http.js'

  export default {
    data () {
      return {
      }
    },
    mounted () {
      // 函数执行 请求接口--微剧列表（GET）
      this.getCategory(1, 0, 40)
    },
    methods: {
      // 函数定义 请求接口--GET
      async getCategory (sortType, startId, pageSize) {
        let header = {
          'content-type': 'application/json' // 默认值
        }
        // 封装参数集合
        let params = {
          sortType: sortType,
          startId: startId,
          pageSize: pageSize
        }
        const newparams = Object.assign(params)
        // 开始执行封装的请求
        const data = await get('/microvision/getMicrovisionListForSquare', newparams, header)
        // 打印出返回的数据
        console.log(data)
      }
    }
  }
</script>

<style scoped>

</style>
