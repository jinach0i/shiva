<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import * as THREE from 'three';
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
let shiba, controls;
const container = ref(null);
onMounted(() => {
  const scene = new THREE.Scene();
  const camera = new THREE.PerspectiveCamera(30,1);
  camera.position.set(0,0,10);
  scene.background=new THREE.Color('skyblue');
  const light=new THREE.DirectionalLight(0xffff00, 10);
  scene.add(light);
  
  const renderer = new THREE.WebGLRenderer({
    antialias: true,
    canvas : document.querySelector('#canvas')
  });
  // gltf용 물체 밝게: renderer.outputEncoding=THREE.sRGBEncoding;
  renderer.setSize(window.innerWidth, window.innerHeight);
  container.value.appendChild(renderer.domElement);
  controls = new OrbitControls(camera, renderer.domElement);
  const loader = new GLTFLoader();
  loader.load("/scene.gltf", (gltf) => {
    shiba = gltf.scene;
    scene.add(shiba);
    console.log(gltf);
});

function animate() {
    requestAnimationFrame(animate);
    // ㄴ옛날에 game만들 때 쓰곤 한&괄호 안 code를 1초에(=frame마다) 60번 run	
    // 자동회전:
    // if (shiba) {
    //   shiba.rotation.y -= 0.1;
    //   shiba.rotation.x -= 0.01;
    // }
    controls.update();
    //회전 시킨걸)render:
    renderer.render(scene, camera);
  }
animate();
onUnmounted(() => {
    renderer.dispose();
    controls.dispose();
  });
});
</script>
<template>
  <div>
    <div ref="container"></div>
    <canvas id="canvas" width="300" height="300"></canvas>
    asd
  </div>
</template>
<style scoped>
*{margin: 0; padding: 0; box-sizing: border-box;}
#container {
  width: 100%;
  height: 100vh;
}
</style>