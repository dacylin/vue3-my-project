<template>
  <div id="app">
    <h1>郵遞區號查詢</h1>
    <ZipcodeSelector />
  </div>
</template>

<script>
import ZipcodeSelector from './components/ZipcodeSelector.vue';

export default {
  components: {
    ZipcodeSelector,
  },
};
</script>

<style>
h1 {
  text-align: center;
  margin: 20px 0;
}
</style>
