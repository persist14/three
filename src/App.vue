<!--
 * @Description: 
 * @Author: 
 * @Date: 2023-10-11 09:11:00
 * @LastEditTime: 2023-10-11 09:58:38
 * @LastEditors: Please set LastEditors
 * @Reference: 
-->
<script setup lang="ts">
import * as THREE from "three";
import { OrbitControls } from "three/addons/controls/OrbitControls.js";
const camera = new THREE.PerspectiveCamera(
	70,
	window.innerWidth / window.innerHeight,
	0.01,
	10
);
camera.position.y = 1;
camera.position.z = 3;

const scene = new THREE.Scene();

const geometry = new THREE.BoxGeometry(0.2, 0.2, 0.2);
const material = new THREE.MeshNormalMaterial();

const mesh = new THREE.Mesh(geometry, material);
mesh.position.set(2, 1, 0);
scene.add(mesh);
const gridHelper = new THREE.GridHelper(10, 10);
scene.add(gridHelper);
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.setAnimationLoop(animation);
// 轨道控制器
const controls = new OrbitControls(camera, renderer.domElement);
// 监听事件
// controls.addEventListener("change", function () {
// 	console.log(111);
// });
// 开启阻尼
controls.enableDamping = true;
// 自动旋转
controls.autoRotate = true;
controls.autoRotateSpeed = 1;
// 添加坐标轴
const axesHelper = new THREE.AxesHelper(5);
axesHelper.position.y = 1;
scene.add(axesHelper);
controls.update();
document.body.appendChild(renderer.domElement);

// animation

function animation(time: number) {
	mesh.rotation.x = time / 2000;
	mesh.rotation.y = time / 1000;
	controls.update();
	renderer.render(scene, camera);
}
animation(200);
</script>

<template>
	<div></div>
</template>

<style scoped></style>
