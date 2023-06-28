<template>
  <div id="app">
    <VueMatrixRaindrop></VueMatrixRaindrop>
    <!-- <VueMatrix></VueMatrix> -->
  </div>
</template>

<script>

import VueMatrixRaindrop from './view/vue-matrix-raindrop'
// import VueMatrix from './view/vue-matrix-017'
export default {
  name: 'App',
  components:{
    VueMatrixRaindrop,
    // VueMatrix
  }
}
</script>