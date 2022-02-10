<template>
  <span class="app">
    <div class="glass">
      <h1 class="title">NEW ADVENTURES</h1>
    </div>
    <img
      class="foreground"
      ref="foreground"
      src="./../public/assets/forest.png"
    />
    <div class="containerText">
      <div>
      <h1 class="title">Coucou</h1>
      <p>
        "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
        tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim
        veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea
        commodo consequat. Duis aute irure dolor in reprehenderit in voluptate
        velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint
        occaecat cupidatat non proident, sunt in culpa qui officia deserunt
        mollit anim id est laborum."
      </p>
      </div>
    </div>
    <Renderer ref="rendererC" antialias resize="window" alpha="true">
      <Camera :position="{ z: 6, x: 0.5, y: 1 }" />
      <Scene>
        <Group
          ref="containerRocket"
          :position="{ x: -7, y: -2, z: 2 }"
          :rotation="{ z: (15 * Math.PI) / 12 }"
        >
          <Group
            ref="groupRocket"
            :position="{ x: -5, y: 0, z: -7 }"
            :rotation="{}"
          >
            <GltfModel
              ref="modelC"
              src="../public/models/rocket/rocket.gltf"
              @load="onReady"
            />
          </Group>
        </Group>
        <HemisphereLight :position="{ y: 50, z: 50 }" />
        <Group
          ref="groupC"
          :position="{ x: 3, y: 2, z: 0 }"
          :rotation="{ y: (9 * Math.PI) / 6, x: (11 * Math.PI) / 8 }"
        >
          <Sphere
            :radius="1.1"
            ref="meshC"
            :widthSegments="64"
            :heightSegments="32"
          >
            <ToonMaterial color="#B0E5DF" />
          </Sphere>
          <Sphere
            :radius="0.2"
            ref="orbitC"
            :position="{ x: 1.2, y: 1.2, z: 1.2 }"
          >
            <ToonMaterial color="#D77B86" />
          </Sphere>
        </Group>
        <Ring
          :innerRadius="1.5"
          :outerRadius="2.8"
          :thetaSegments="30"
          :phiSegments="2"
          :position="{ x: 3, y: 2, z: 0 }"
          :rotation="{ y: (-6.6 * Math.PI) / 6, x: (5.2 * Math.PI) / 8 }"
        >
          <ToonMaterial :props="{ side: 2 }" color="#EADEE7" />
        </Ring>
        <Ring
          ref="lineC"
          :innerRadius="1.2"
          :outerRadius="1.3"
          :thetaSegments="30"
          :phiSegments="2"
          :thetaLength="6.1"
          :position="{ x: 3, y: 2, z: 0 }"
          :rotation="{ y: (-6.6 * Math.PI) / 6, x: (5.2 * Math.PI) / 8 }"
        >
          <ToonMaterial :props="{ side: 2 }" color="#EADEE7" />
        </Ring>
        <Sphere
          :position="{
            x: generateRandom(-500, 500),
            y: generateRandom(-30, 500),
            z: generateRandom(-1000, -300),
          }"
          :ref="`box${index}`"
          v-for="(i, index) in 400"
          :key="index"
        >
          <ToonMaterial />
        </Sphere>
      </Scene>
    </Renderer>
  </span>
</template>

<script setup>

import { ref, onMounted, onBeforeUnmount } from "vue";
const rendererC = ref();
const meshC = ref();
const lineC = ref();
const orbitC = ref();
const groupC = ref();
const modelC = ref();
const groupRocket = ref();
let valueScroll=0

onMounted(() => {
  window.addEventListener('scroll', handleScroll);
  const renderer = rendererC.value;
  const sphere = meshC.value.mesh;
  const line = lineC.value.mesh;
  const orbit = orbitC.value.mesh;
  const group = groupC.value.group;
  const rocket = groupRocket.value.group;
  const model = modelC.value.scene;
  renderer.renderer.setClearColor(0x000000, 0);
  renderer.onBeforeRender(() => {
    sphere.rotation.z += 0.01;
    line.rotation.z += 0.01;
    orbit.rotation.z += 0.01;
    group.rotation.y += 0.01;
    if (model) {
      rocket.rotation.y += 0.002;
    }
  });
})

onBeforeUnmount(() => {
  window.removeEventListener('scroll', handleScroll);
})
function generateRandom(min = 20, max = 50) {
  let difference = max - min;
  // generate random number
  let rand = Math.random();
  // multiply with difference
  rand = Math.floor(rand * difference);
  rand = rand + min;
  return rand;
}

function handleScroll(e) {
  const rocket = groupRocket.value.group;
  const distance = window.scrollY
  if(distance > valueScroll) rocket.position.y += 0.05
  else rocket.position.y -= 0.05
  rocket.rotation.y += 0.05;
  valueScroll = distance
}

function onReady(model) {
  model.traverse((child) => {
    if (child.isMesh) {
      if (child.name === "mesh_0") {
        child.material.color.r = 0.917;
        child.material.color.g = 0.87;
        child.material.color.b = 0.905;
      }
      if (child.name === "mesh_1") {
        child.material.color.r = 0.69;
        child.material.color.g = 0.898;
        child.material.color.b = 0.874;
      }
      if (child.name === "mesh_3") {
        child.material.color.r = 0.69;
        child.material.color.g = 0.898;
        child.material.color.b = 0.874;
      }
      if (child.name === "mesh_3") {
        child.material.color.r = 0.69;
        child.material.color.g = 0.898;
        child.material.color.b = 0.874;
      }
      if (child.name === "mesh_4") {
        child.material.color.r = 0.69;
        child.material.color.g = 0.898;
        child.material.color.b = 0.874;
      }
    }
  });
}
</script>

<style>
html {
  width: 100%;
  height: 100%;
}
body {
  width: 100%;
  height: 100%;
  margin: 0;
  background: linear-gradient(#194D47, #D8F3EF);
}
.app {
  display: flex;
  flex-direction: column;
}
canvas {
  width: 100%;
  height: 100vh;
  display: block;
  position: fixed;
  top: 0;
  left: 0;
  z-index: -10;
}
.glass {
  display: flex;
  justify-content: center;
  align-content: center;
  align-items: center;
  margin: auto;
  margin-top: 14rem;
  min-height: 100px;
  min-width: 700px;
  box-shadow: 0 0 1rem 0 rgba(0, 0, 0, 0.2);
  border-radius: 5px;
  background-color: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(5px);
  border-radius: 9px;
}
.title {
  font-size: 3rem;
  font-weight: 900;
  color: #f3e6f0;
}
.containerText{
  background-color: #194D47;
  height: 100vh;
  width: 100vw;
  padding: 5rem;
}
</style>