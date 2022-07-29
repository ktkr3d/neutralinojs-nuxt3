<template>
  <div>
    <div id="overlay">
      <button id="startButton">Play</button>
    </div>
    <div id="info">
      <a href="https://threejs.org" target="_blank" rel="noopener">three.js</a> - MMDLoader test<br />
      <a href="https://github.com/mrdoob/three.js/tree/master/examples/models/mmd#readme" target="_blank" rel="noopener">MMD Assets license</a><br />
      Copyright
      <a href="https://sites.google.com/view/evpvp/" target="_blank" rel="noopener">Model Data</a>
      <a href="http://www.nicovideo.jp/watch/sm13147122" target="_blank" rel="noopener">Dance Data</a>
      <a href="http://www.nicovideo.jp/watch/sm11938255" target="_blank" rel="noopener">Audio Data</a><br />
      Camera is customized from <a href="http://www.nicovideo.jp/watch/sm19168559" target="_blank" rel="noopener">this Data</a>
    </div>
  </div>
</template>

<script type="module">
import * as THREE from "three";

import { MMDAnimationHelper } from "three/examples/jsm/animation/MMDAnimationHelper.js";
import { OutlineEffect } from "three/examples/jsm/effects/OutlineEffect.js";
import { MMDLoader } from "three/examples/jsm/loaders/MMDLoader.js";

export default {
  mounted() {
    let mesh, camera, scene, renderer, effect;
    let helper;
    const vm = this;
    let ready = false;

    const clock = new THREE.Clock();

    //const startButton = document.getElementById( 'startButton' );
    //startButton.addEventListener('click', function () {
    //Ammo().then(function () {
    init();
    animate();
    //});
    //});

    function init() {
      //const overlay = document.getElementById("overlay");
      //overlay.remove();

      const container = document.createElement("div");
      document.body.appendChild(container);

      camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 1, 2000);

      // scene

      scene = new THREE.Scene();
      scene.background = new THREE.Color(0xffffff);

      scene.add(new THREE.PolarGridHelper(30, 10));

      const listener = new THREE.AudioListener();
      camera.add(listener);
      scene.add(camera);

      const ambient = new THREE.AmbientLight(0x666666);
      scene.add(ambient);

      const directionalLight = new THREE.DirectionalLight(0x887766);
      directionalLight.position.set(-1, 1, 1).normalize();
      scene.add(directionalLight);

      //

      renderer = new THREE.WebGLRenderer({ antialias: true });
      renderer.setPixelRatio(window.devicePixelRatio);
      renderer.setSize(window.innerWidth, window.innerHeight);
      container.appendChild(renderer.domElement);

      effect = new OutlineEffect(renderer);

      // model

      function onProgress(xhr) {
        if (xhr.lengthComputable) {
          const percentComplete = (xhr.loaded / xhr.total) * 100;
          console.log(Math.round(percentComplete, 2) + "% downloaded");
        }
      }

      //const modelFile = "miku/miku_v2.pmd";
      //const modelFile = "miku/lat_miku_v2.pmd";
      //const modelFile = "neen_mmd_ver1.0/PMX/nene_STD.pmx";
      const modelFile = "PMX/nene_BEA.pmx";
      const vmdFiles = ["vmds/wavefile_v2.vmd"];
      const cameraFiles = ["vmds/wavefile_camera.vmd"];
      const audioFile = "audios/wavefile_short.mp3";
      const audioParams = { delayTime: (160 * 1) / 30 };

      helper = new MMDAnimationHelper();

      const loader = new MMDLoader();

      loader.loadWithAnimation(
        modelFile,
        vmdFiles,
        function (mmd) {
          mesh = mmd.mesh;

          helper.add(mesh, {
            animation: mmd.animation,
            //physics: true,
            physics: false,
          });

          loader.loadAnimation(
            cameraFiles,
            camera,
            function (cameraAnimation) {
              helper.add(camera, {
                animation: cameraAnimation,
              });

              new THREE.AudioLoader().load(
                audioFile,
                function (buffer) {
                  const audio = new THREE.Audio(listener).setBuffer(buffer);

                  helper.add(audio, audioParams);
                  scene.add(mesh);

                  ready = true;
                },
                onProgress,
                null
              );
            },
            onProgress,
            null
          );
        },
        onProgress,
        null
      );

      //

      window.addEventListener("resize", onWindowResize);
    }

    function onWindowResize() {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();

      effect.setSize(window.innerWidth, window.innerHeight);
    }

    //

    function animate() {
      requestAnimationFrame(animate);
      render();
    }

    function render() {
      if (ready) {
        helper.update(clock.getDelta());
      }

      effect.render(scene, camera);
    }
  },
};
</script>

<style>
body {
  color: #444;
}
a {
  color: #08f;
}

#info {
	position: absolute;
	top: 0px;
	width: 100%;
	padding: 10px;
	box-sizing: border-box;
	text-align: center;
	-moz-user-select: none;
	-webkit-user-select: none;
	-ms-user-select: none;
	user-select: none;
	pointer-events: none;
	z-index: 1; /* TODO Solve this in HTML */
}

#overlay {
	position: absolute;
	font-size: 16px;
	z-index: 2;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
	display: flex;
	align-items: center;
	justify-content: center;
	flex-direction: column;
	background: rgba(0,0,0,0.7);
}

	#overlay button {
		background: transparent;
		border: 0;
		border: 1px solid rgb(255, 255, 255);
		border-radius: 4px;
		color: #ffffff;
		padding: 12px 18px;
		text-transform: uppercase;
		cursor: pointer;
	}
</style>
