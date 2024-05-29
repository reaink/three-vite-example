<script setup lang="ts">
import { onMounted } from "vue";
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/Addons.js";

onMounted(() => {
  const scene = new THREE.Scene();

  // scene.background = new THREE.Color(0x666666);

  const path = "/public/";
  const ext = ".jpg";
  const urls = [
    path + "px" + ext,
    path + "nx" + ext,
    path + "py" + ext,
    path + "ny" + ext,
    path + "pz" + ext,
    path + "nz" + ext,
  ];
  scene.background = new THREE.CubeTextureLoader().load(urls);

  scene.fog = new THREE.Fog(0xcccccc, 10, 15);

  const camera = new THREE.PerspectiveCamera();
  camera.position.y = 5;
  camera.position.z = 10;

  const geometry = new THREE.BoxGeometry(1, 1, 1);

  const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });

  const mesh = new THREE.Mesh(geometry, material);
  mesh.position.set(0, 3, 0);
  scene.add(mesh);

  const gridHelper = new THREE.GridHelper(10, 10);
  scene.add(gridHelper);

  const renderer = new THREE.WebGLRenderer();
  renderer.setSize(window.innerWidth, window.innerHeight);

  document.getElementById("container")!.appendChild(renderer.domElement);

  const controls = new OrbitControls(camera, renderer.domElement);

  controls.dampingFactor = 0.03;
  controls.enableDamping = true;

  // controls.enableAutoRotate = true;
  controls.autoRotate = true;
  controls.autoRotateSpeed = 10;

  // const axesHelper = new THREE.AxesHelper(5);
  // axesHelper.position.y = 3;
  // scene.add(axesHelper);

  // renderer.render(scene, camera);

  function animate() {
    requestAnimationFrame(animate);

    // mesh.rotation.x += 0.01;
    // mesh.rotation.y += 0.01;

    controls.update();
    renderer.render(scene, camera);
  }

  animate();
});
</script>

<template>
  <div id="container"></div>
</template>

<style scoped></style>
