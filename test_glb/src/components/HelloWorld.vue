<template>
  <canvas class="canvas" id="canvas"></canvas>
</template>

<script>
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
import { reactive, onMounted } from 'vue';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },

  setup() {
    onMounted(() => {
      const canvas = document.querySelector(".canvas");

      const scene = new THREE.Scene();

      const camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        0.1,
        10000
      );
      camera.position.z = 1000;

      const renderer = new THREE.WebGLRenderer({
        canvas: canvas
      });
      renderer.setSize(window.innerWidth, window.innerHeight);
      renderer.setClearColor(0x000000, 1)

      const geometry = new THREE.BoxGeometry(1000, 1, 1000);
      const material = new THREE.MeshBasicMaterial({ color: "white" });
      const cube = new THREE.Mesh(geometry, material);
      cube.castShadow = true;
      cube.receiveShadow = true;
      scene.add(cube);

      const controls = new OrbitControls(camera, canvas);
      // controls.target.set(0,0,0);
      controls.enableDamping = true; // Enable smooth camera movements

      // Create a directional light
      const light = new THREE.DirectionalLight(0xffffff, 1);
      light.position.set(10, 10, -10); // Set the light position
      light.intensity = 2
      scene.add(light)

      const loader = new GLTFLoader();
      loader.load('/test3.glb', (gltf) => {
        gltf.scene.castShadow = true;
        gltf.scene.receiveShadow = true;
        scene.add(gltf.scene);
      });
      console.log(scene)
      function animate() {
        requestAnimationFrame(animate);

        controls.update(); // Update the controls

        renderer.render(scene, camera);
      }

      animate();
    });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.canvas {
  width: 100%;
  height: 100%;
}
</style>
