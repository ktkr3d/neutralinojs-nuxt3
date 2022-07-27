<template>
  <div class="animation"></div>
</template>

<script>
import * as THREE from "three";

export default {
  mounted() {
    let scene, renderer, camera;
    const vm = this;

    function init() {
      scene = new THREE.Scene();
      scene.background = new THREE.Color(0x000000);

      const light = new THREE.DirectionalLight(0xff80ff);
      light.intensity = 1;
      light.position.set(2, 4, 0);
      scene.add(light);

      const ambient = new THREE.AmbientLight(0x80ffff);
      ambient.intensity = 0.2;
      scene.add(ambient);

      const geometry = new THREE.BoxGeometry(2, 2, 2);
      const material = new THREE.MeshStandardMaterial({ color: 0x8080ff });
      const mesh = new THREE.Mesh(geometry, material);
      const axis = new THREE.AxesHelper(200);
      mesh.add(axis);
      scene.add(mesh);

      renderer = new THREE.WebGLRenderer();
      renderer.setPixelRatio(window.devicePixelRatio);
      renderer.setSize(window.innerWidth, window.innerWidth);

      const container = vm.$el;
      container.appendChild(renderer.domElement);

      camera = new THREE.PerspectiveCamera(70, window.innerWidth / window.innerHeight, 0.001, 500);
      camera.position.set(3, 3, 3);
      camera.lookAt(mesh.position);

      resize();
    }

    function resize() {
      var w = window.innerWidth;
      var h = window.innerHeight;
      renderer.setSize(w, h);
      camera.aspect = w / h;
      camera.updateProjectionMatrix();
    }

    function animate() {
      requestAnimationFrame(animate);
      render();
    }

    function render() {
      renderer.render(scene, camera);
    }

    init();
    animate();
  },
};
</script>

<style>
.animation {
  position: fixed;
}
</style>
