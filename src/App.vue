<template>
  <div id="cesiumContainer"></div>
</template>
<script setup>
import * as Cesium from "cesium";
import { onMounted } from "vue";

onMounted(() => {
  const config = {
    showRenderLoopErrors: false, //如果为true，则在发生渲染循环错误时，此小部件将自动向包含错误的用户显示HTML面板。
    requestRenderMode: true, // 开启请求的渲染模式
    maximumRenderTimeChange: Infinity, // 处理模拟时间改变
    animation: false, // 是否创建动画小器件，左下角仪表
    baseLayerPicker: false, // 是否显示图层选择器
    fullscreenButton: false, // 是否显示全屏按钮
    geocoder: false, // 是否显示geocoder小器件，右上角查询按钮
    homeButton: false, // 是否显示Home按钮
    infoBox: false, // 是否显示信息框
    shouldAnimate: true, // 允许动画
    sceneModePicker: false, // 是否显示3D/2D选择器
    selectionIndicator: false, // 是否显示选取指示器组件鼠标绿色框
    timeline: false, // 是否显示时间轴
    navigationHelpButton: false, // 是否显示右上角的帮助按钮
    vrButton: false, // 是否显示双屏
    scene3DOnly: true, // 如果设置为true，则所有几何图形以3D模式绘制以节约GPU资源
    fullscreenElement: document.body, // 全屏时渲染的HTML元素
    orderIndependentTranslucency: false,
    navigationInstructionsInitiallyVisible: false,
  };
  const viewer = new Cesium.Viewer("cesiumContainer", config);

  // 将viewer和Cesium挂载到window上
  window.viewer = viewer;
  window.Cesium = Cesium;

  viewer.scene.debugShowFramesPerSecond = true; //不显示帧率
  viewer.shadows = true; //开启或关闭阴影
  // 关闭抗锯齿
  viewer.scene.fxaa = true;
  viewer.scene.postProcessStages.fxaa.enabled = true;
  //帧率检测
  viewer.scene.debugShowFramesPerSecond = false;
  //全球光照
  viewer.scene.globe.enableLighting = false;
  //更改配置，性能优化
  viewer.scene.logarithmicDepthBuffer = true;
  // 取消双击事件-追踪该位置
  viewer.cesiumWidget.screenSpaceEventHandler.removeInputAction(
    Cesium.ScreenSpaceEventType.LEFT_DOUBLE_CLICK
  );

  // 去除版权信息
  viewer._cesiumWidget._creditContainer.style.display = "none";
});
</script>

<style>
html,
body,
#app,
#cesiumContainer {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
  overflow: hidden;
}
</style>
