<template>
  <span class="app">
    <div class="glass">
      <h1 class="title">NEW ADVENTURES</h1>
    </div>

    <Renderer ref="rendererC" antialias resize="window">
      <Camera :position="{ z: 6, x: 0.5, y: 1 }" />
      <Scene :fog="('#BBC7C5', 0.0025, 20)">
          <Group
            ref="groupRocket"
            :position="{ x: -5, y: 0, z: -7 }"
            :rotation="{z:(-1 * Math.PI) / 6}">
            <GltfModel
              ref="modelC"
              src="../public/models/rocket/rocket.gltf"
              @load="onReady"
            />
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
      </Scene>
    </Renderer>
  </span>
</template>

<script setup>
import { ref, onMounted } from "vue";
const rendererC = ref();
const meshC = ref();
const lineC = ref();
const orbitC = ref();
const groupC = ref();
const modelC = ref();
const groupRocket = ref();
onMounted(() => {
  const renderer = rendererC.value;
  const sphere = meshC.value.mesh;
  const line = lineC.value.mesh;
  const orbit = orbitC.value.mesh;
  const group = groupC.value.group;
  const groupR = groupRocket.value.group;
  const model = modelC.value.scene;
  renderer.color = "#B0E5DF";
  renderer.renderer.setClearColor("#BBC7C5");
  renderer.onBeforeRender(() => {
    sphere.rotation.z += 0.01;
    line.rotation.z += 0.01;
    orbit.rotation.z += 0.01;
    group.rotation.y += 0.01;
    if (model) {
      groupR.rotation.y += 0.001;
    }
  });
});
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
body {
  margin: 0;
}
.app {
  display: flex;
}
canvas {
  width: 100%;
  height: 100vh;
  display: block;
  position: fixed;
  top: 0;
  left: 0;
  z-index: -9999;
}
.glass {
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
  display: flex;
  justify-content: center;
  align-content: center;
  align-items: center;
  font-size: 3em;
  font-weight: 900;
  color: #f3e6f0;
}
</style>