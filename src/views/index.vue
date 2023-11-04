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
  // color: "#00ffff",
  wireframe: false,
};
// 创建实例
const gui = new dat.GUI();
const f = gui.addFolder("配置");
f.add(controlData, "rotateSpeed").min(0.01).max(0.1).step(0.01);
// f.addColor(controlData, "color");
f.add(controlData, "wireframe");
// 绑定元素id
f.domElement.id = "gui";
f.open();

const camera = new THREE.PerspectiveCamera();
// 70,
// window.innerWidth / window.innerHeight,
// 0.01,
// 10
camera.position.y = 3;
camera.position.z = 10;
camera.position.x = 3;

const scene = new THREE.Scene();
// 添加背景颜色
// scene.background = new THREE.Color(0x666666)

// 创建纹理
// const texture = new THREE.TextureLoader().load("./src/img/bg.jpg");

// 添加背景图片
const cubeTexture = new THREE.CubeTextureLoader()
  .setPath("./src/img/")
  .load([
    "test2.jpg",
    "test2.jpg",
    "test2.jpg",
    "test2.jpg",
    "test2.jpg",
    "test2.jpg",
  ]);
scene.background = cubeTexture;

// 创建立方体
// const geometry = new THREE.BoxGeometry(0.2, 0.2, 0.2);
// const material = new THREE.MeshBasicMaterial({
//   map: texture,
// });

// 创建球体
// const geometry = new THREE.SphereGeometry(0.3);
// const material = new THREE.MeshBasicMaterial({
//   // color: 0x00ff00,
//   envMap: cubeTexture,
// });

// const mesh = new THREE.Mesh(geometry, material);

// 创建平面几何体
// const geometry = new THREE.PlaneGeometry(1, 1);
// const material = new THREE.MeshBasicMaterial({
//   color: 0xffff00,
//   side: THREE.DoubleSide,
//   wireframe: true,
// });
// const plane = new THREE.Mesh(geometry, material);
// scene.add(plane);
// const geometry = new THREE.BufferGeometry();
// // 创建一个简单的矩形. 在这里我们左上和右下顶点被复制了两次。
// // 因为在两个三角面片里，这两个顶点都需要被用到。
// const vertices = new Float32Array([
//   -1.0, -1.0, 1.0, 1.0, -1.0, 1.0,
//   // 1.0, 1.0, 1.0,
//   1.0, 1.0, 1.0, -1.0, 1.0, 1.0,
//   //  -1.0, -1.0, 1.0,
// ]);
// const indexs = new Uint16Array([0, 1, 2, 2, 3, 0]);
// geometry.index = new THREE.BufferAttribute(indexs, 1);
// // itemSize = 3 因为每个顶点都是一个三元组。
// geometry.setAttribute("position", new THREE.BufferAttribute(vertices, 3));
// const material = new THREE.MeshBasicMaterial({
//   color: 0xff0000,
//   wireframe: true,
// });
// const mesh = new THREE.Mesh(geometry, material);
// scene.add(mesh);

// 创建矩形
// const geometry = new THREE.PlaneGeometry(1, 1);

// // 创建纹理对象
// const texture = new THREE.TextureLoader().load("./src/img/bg.jpg");
// // 定义uv
// console.log(geometry, ">>>");

// const uv = new Float32Array([0.5, 1, 1, 1, 0.5, 0.5, 1, 0.5]);
// geometry.attributes.uv = new THREE.BufferAttribute(uv, 2);
// // 创建基础材质
// const material = new THREE.MeshBasicMaterial({
//   map: texture,
//   // wirefirame: true,
// });
// // 创建网格
// const mesh = new THREE.Mesh(geometry, material);
// scene.add(mesh);
const geometry = new THREE.BoxGeometry(1, 1, 1);
const material = new THREE.MeshPhongMaterial({
  color: 0x0099ff,
  shininess: 1000,
});

const cube = new THREE.Mesh(geometry, material);
cube.position.set(0, 0.5, 0);

// 物体接受光源
cube.receiveShadow = true;
// 物体投射光源
cube.castShadow = true;

scene.add(cube);
// 添加光源
// 环境光源
const light = new THREE.AmbientLight(0xfffff, 1); // 柔和的白光
scene.add(light);
// 点光源
const pointLight = new THREE.PointLight(0xffff, 100, 100);
pointLight.position.set(5, 3, 5);
pointLight.castShadow = true;
scene.add(pointLight);

// 创建地面
const floor = new THREE.Mesh(
  new THREE.PlaneGeometry(10, 10),
  new THREE.MeshPhongMaterial({
    color: 0x1b5e20,
  })
);
floor.rotation.x -= Math.PI / 2;
floor.receiveShadow = true;
scene.add(floor);
// mesh.position.set(0, 1, 0);
// scene.add(mesh);
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
  renderer.shadowMap.enabled = true;
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
    // mesh.rotation.x += controlData.rotateSpeed;
    // mesh.rotation.y += controlData.rotateSpeed;
    // mesh.material.color = new THREE.Color(controlData.color);
    // material.wireframe = controlData.wireframe;
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
