<template>
  <div class="container">
    <AMap />
  </div>
</template>

<script lang="ts" setup>
  import AMap from '@/components/amap/index.vue';
</script>

<script lang="ts">
  export default {
    name: 'Dashboard', // If you want the include property of keep-alive to take effect, you must name the component
  };
</script>

<style lang="less" scoped>
  .container {
    background-color: var(--color-fill-2);
    padding: 16px 20px;
    padding-bottom: 0;
    display: flex;
    width: 600px;
    height: 600px;
  }
</style>
