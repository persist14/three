<template>
  <div id="container"></div>
</template>

<script setup lang="ts">
import { OrbitControls } from "three/addons/controls/OrbitControls.js";
import { onMounted } from "vue";
import * as THREE from "three";

const scene = new THREE.Scene();
// 添加背景颜色
// scene.background = new THREE.Color();
// 创建相机
const camera = new THREE.PerspectiveCamera();
camera.position.y = 1;
camera.position.z = 10;

const geometry = new THREE.BoxGeometry(1, 1, 1);
const material = new THREE.MeshBasicMaterial({
  color: 0x00ff00,
});
const cube = new THREE.Mesh(geometry, material);
const vector = new THREE.Vector3(1, 2, 3);
cube.position.add(vector);
scene.add(cube);

onMounted(() => {
  const renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setSize(window.innerWidth, window.innerHeight);
  renderer.setAnimationLoop(animation);
  const controls = new OrbitControls(camera, renderer.domElement);
  // 开启阻尼
  controls.enableDamping = true;
  // 自动旋转
  controls.autoRotate = true;
  controls.autoRotateSpeed = 1;
  const axesHelper = new THREE.AxesHelper(5);

  scene.add(axesHelper);
  controls.update();

  document.getElementById("container")?.appendChild(renderer.domElement);
  function animation() {
    controls.update();
    renderer.render(scene, camera);
  }
  animation();
});
</script>

<style scoped></style>
