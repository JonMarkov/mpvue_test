<template>
  <div>

  </div>
</template>

<script>
  import {post} from '@/utils/http.js'

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
        const data = await post('/cms/news/list')
        this.category = data.data
        console.log(data)
      }
    }
  }
</script>

<style>

</style>
