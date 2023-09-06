<script setup lang="ts">
import * as THREE from 'three'
import { onMounted, ref } from 'vue'
import { useWindowSize } from '@vueuse/core'

const scene = new THREE.Scene()

const renderer = new THREE.WebGLRenderer()

const rubiksCube = ref<HTMLDivElement | null>(null)
onMounted(() => {
  const elSize = useWindowSize()
  const width = elSize.width.value
  const height = elSize.height.value
  renderer.setSize(width, height)
  let camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000)
  camera.position.z = 10
  rubiksCube.value?.appendChild(renderer.domElement)
  const geometry = new THREE.BoxGeometry(1, 1, 1)
  const material = new THREE.MeshBasicMaterial({ color: '#967431' })
  const cube = new THREE.Mesh(geometry, material)
  scene.add(cube)
  function animate() {
    const newSize = useWindowSize()
    renderer.setSize(newSize.width.value, newSize.height.value)
    camera = new THREE.PerspectiveCamera(75, newSize.width.value / newSize.height.value, 0.1, 1000)
    camera.position.z = 10
    requestAnimationFrame(animate)
    cube.rotation.x += 0.01
    cube.rotation.y += 0.01
    renderer.render(scene, camera)
  }
  animate()
})
</script>

<template>
  <div id="rubiks-cube" ref="rubiksCube" class="w-screen h-screen" />
</template>

<style scoped>
</style>
