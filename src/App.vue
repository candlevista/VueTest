<template>
  <div>
    <v-header :seller="seller"></v-header>
  </div>
</template>

<script>
import header from 'components/header/header.vue'
import { getSeller } from 'api/index.js'

export default {
  data() {
    return {
      seller: {}
    }
  },
  created() {
    this.getSeller()
  },
  methods: {
    getSeller() {
      getSeller().then(data => {
        this.seller = Object.assign({}, this.seller, data)
      })
    }
  },
  components: {
    'v-header': header
  }
}
</script>

<style>
</style>
