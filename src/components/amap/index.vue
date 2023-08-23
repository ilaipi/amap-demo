<template>
  <div id="container"></div>
</template>

<script lang="ts" setup>
  import { shallowRef } from 'vue';
  import AMapLoader from '@amap/amap-jsapi-loader';

  /* eslint-disable no-underscore-dangle */
  window._AMapSecurityConfig = {
    securityJsCode: '6658a10e891653dff6fc99205c61e4ad',
  };
  /* eslint-enable no-underscore-dangle */

  const map = shallowRef(null);

  const loadAMap = async () => {
    const AMap = await AMapLoader.load({
      key: '167d6165637d70adbd78516de83125d7',
      // version: '1.4.15',
      version: '2.0',
      plugins: [
        'AMap.ToolBar',
        'AMap.Scale',
        'AMap.HawkEye',
        'AMap.Geolocation',
      ],
    });
    map.value = new AMap.Map('container', {
      zoom: 11,
      center: [116.397428, 39.90923],
    });
    AMap.plugin('AMap.ToolBar', () => {
      if (map.value) {
        (map.value as any).addControl(new AMap.ToolBar());
      }
    });
    AMap.plugin('AMap.Scale', () => {
      if (map.value) {
        (map.value as any).addControl(new AMap.Scale());
      }
    });
    AMap.plugin('AMap.HawkEye', () => {
      console.log('= plugin HawkEye==');
      if (map.value) {
        (map.value as any).addControl(new AMap.HawkEye());
      }
    });
    AMap.plugin('AMap.Geolocation', () => {
      const geolocation = new AMap.Geolocation({
        enableHighAccuracy: true, // 是否使用高精度定位，默认：true
        timeout: 10000, // 设置定位超时时间，默认：无穷大
        offset: [10, 20], // 定位按钮的停靠位置的偏移量
        zoomToAccuracy: true, //  定位成功后调整地图视野范围使定位位置及精度范围视野内可见，默认：false
        position: 'RT', //  定位按钮的排放位置,  RB表示右下
      });

      function onComplete(data: any) {
        // data是具体的定位信息
        console.log('==========location success 1', data);
      }

      function onError(data: any) {
        // 定位出错
        console.log('==========location error', data);
      }

      geolocation.getCurrentPosition((status: string, result: any) => {
        if (status === 'complete') {
          onComplete(result);
        } else {
          onError(result);
        }
      });
      if (map.value) {
        (map.value as any).addControl(geolocation);
      }
    });
  };
  loadAMap();
</script>

<style scope>
  #container {
    height: 100%;
    width: 100%;
    padding: 0px;
    margin: 0px;
  }
</style>
