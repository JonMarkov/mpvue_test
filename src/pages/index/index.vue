<template>
  <div>

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
      this.getCategory()
    },
    methods: {
      async getCategory () {
        const data = await get('/shop/goods/category/all')
        this.category = data.data
        console.log(data)
      }
    }
  }
</script>

<style scoped>

</style>
