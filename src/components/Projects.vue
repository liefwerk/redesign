<template lang="">
<div></div>
</template>

<script>
// import { THREE } from 'three'
// var THREE = require('three')
import * as THREE from 'three'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'

export default {
  data () {
    return {}
  },
  methods: {
    loadObj: function () {
      var scene = new THREE.Scene()
      var camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      )
      var intensity = 2
      var width = 1000
      var height = 1000
      var rectLight = new THREE.RectAreaLight(
        0xffffff,
        intensity,
        width,
        height
      )
      rectLight.position.set(50, 50, 0)
      rectLight.lookAt(0, 0, 0)
      var renderer = new THREE.WebGLRenderer()
      var controls = new OrbitControls(camera, renderer.domElement)
      // controls.update() must be called after any manual changes to the camera's transform
      camera.position.set(0, 20, 100)
      controls.update()
      renderer.setClearColor(0xffe0d3ed, 1)
      var loader = new GLTFLoader()
      loader.load(
        './piggy_girl.gltf',
        function (gltf) {
          scene.add(gltf.scene, rectLight)
        },
        undefined,
        function (error) {
          console.error(error)
        }
      )

      camera.position.z = 200
      const innerwidth = window.innerWidth || document.documentElement.clientWidth ||
document.body.clientWidth
      const innerheight = window.innerHeight || document.documentElement.clientHeight ||
document.body.clientHeight
      renderer.setSize(innerwidth, innerheight)
      document.body.appendChild(renderer.domElement)

      var animate = function () {
        requestAnimationFrame(animate)

        // cube.rotation.x += 0.01
        // cube.rotation.y += 0.01
        // required if controls.enableDamping or controls.autoRotate are set to true
        controls.update()

        renderer.render(scene, camera)
      }

      animate()
    }
  },
  beforeMount () {
    this.loadObj()
  }
}
</script>
<style lang="css">
#canvas {
  display: block;
}
* {
  margin: 0;
}
</style>
