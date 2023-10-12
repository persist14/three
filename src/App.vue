<!--
 * @Description: 
 * @Author: 
 * @Date: 2023-10-11 09:11:00
 * @LastEditTime: 2023-10-12 14:18:41
 * @LastEditors: Please set LastEditors
 * @Reference: 
-->
<script setup lang="ts">
import * as THREE from "three";
import { OrbitControls } from "three/addons/controls/OrbitControls.js";
import * as dat from "dat.gui";
import { onMounted } from "vue";
// dat.gui配置
// 配置数据
const controlData = {
	rotateSpeed: 0.01,
	color: "#00ffff",
	wireframe: false
};
// 创建实例
const gui = new dat.GUI();
const f = gui.addFolder("配置");
f.add(controlData, "rotateSpeed").min(0.01).max(0.1).step(0.01);
f.addColor(controlData, "color");
f.add(controlData, "wireframe");
// 绑定元素id
f.domElement.id = "gui";
f.open();

const camera = new THREE.PerspectiveCamera();
// 70,
// window.innerWidth / window.innerHeight,
// 0.01,
// 10
camera.position.y = 1;
camera.position.z = 3;

const scene = new THREE.Scene();
// 添加背景颜色
// scene.background = new THREE.Color(0x666666)
// 添加背景图片
// scene.background = new THREE.CubeTextureLoader()
// 	.setPath("./src/img/")
// 	.load(["bg.jpg", "bg.jpg", "bg.jpg", "bg.jpg", "bg.jpg", "bg.jpg"]);
const geometry = new THREE.BoxGeometry(0.2, 0.2, 0.2);
const material = new THREE.MeshBasicMaterial();

const mesh = new THREE.Mesh(geometry, material);

mesh.position.set(0, 0, 0);
scene.add(mesh);
// 添加雾
// scene.fog = new THREE.Fog(0xcccccc, 10, 15);
// const moveCamera = () => {
// 	camera.position.y = 5;
// 	camera.position.z = 0;
// 	camera.lookAt(3, 3, 0);
// };
// const moveCube = () => {
// 	mesh.position.set(2, 2, 0);
// 	camera.lookAt(mesh.position);
// };

const gridHelper = new THREE.GridHelper(10, 10);
scene.add(gridHelper);
onMounted(() => {
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
	// axesHelper.position.y = 1;
	scene.add(axesHelper);
	controls.update();
	// 添加gui到指定的元素
	document.getElementById("container")?.appendChild(f.domElement);
	document.getElementById("container")?.appendChild(renderer.domElement);

	// animation

	function animation() {
		// mesh.rotation.x = time / 2000;
		// mesh.rotation.y = time / 1000;
		mesh.rotation.x += controlData.rotateSpeed;
		mesh.rotation.y += controlData.rotateSpeed;

		mesh.material.color = new THREE.Color(controlData.color);
		material.wireframe = controlData.wireframe;
		controls.update();
		renderer.render(scene, camera);
	}
	animation();
});
</script>

<template>
	<!-- <button @click="moveCamera">移动相机</button>
	<button @click="moveCube">移动物体</button> -->
	<div id="container"></div>
</template>

<style>
#gui {
	position: absolute;
	right: 0;
	width: 300px;
}
</style>
