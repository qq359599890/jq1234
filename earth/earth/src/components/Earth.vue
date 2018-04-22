<template>
  <div class="earth">
    <div id="EarthContainer"></div>
  </div>
</template>

<script>
// import THREE from 'three'
// import CanvasRenderer from '@/assets/js/CanvasRenderer'
// import Projector from '@/assets/js/Projector'

import CanvasRenderer from '@/assets/js/three/CanvasRenderer'
import Projector from '@/assets/js/three/Projector'
var THREE = require('@/assets/js/three/three').THREE

import earthImg from '@/assets/land_ocean_ice_cloud_2048.jpg'

export default {
  name: 'earth',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  mounted: function() {
    var container, stats
    var camera, scene, renderer
    var group
    var mouseX = 0, mouseY = 0
    var windowHalfX = window.innerWidth / 2
    var windowHalfY = window.innerHeight / 2
    init()
    animate()
    function init() {
      container = document.getElementById( 'EarthContainer' )
      camera = new THREE.PerspectiveCamera( 60, window.innerWidth / window.innerHeight, 1, 2000 )
      camera.position.z = 500
      scene = new THREE.Scene()
      scene.background = new THREE.Color( 0xffffff )
      group = new THREE.Group()
      scene.add( group )
      // earth
      var loader = new THREE.TextureLoader()
      loader.load( earthImg, function ( texture ) {
        var geometry = new THREE.SphereGeometry( 200, 20, 20 )
        var material = new THREE.MeshBasicMaterial( { map: texture, overdraw: 0.5 } )
        var mesh = new THREE.Mesh( geometry, material )
        group.add( mesh )
      } )
      // shadow
      var canvas = document.createElement( 'canvas' )
      canvas.width = 128
      canvas.height = 128
      var context = canvas.getContext( '2d' )
      var gradient = context.createRadialGradient(
        canvas.width / 2,
        canvas.height / 2,
        0,
        canvas.width / 2,
        canvas.height / 2,
        canvas.width / 2
      )
      gradient.addColorStop( 0.1, 'rgba(210,210,210,1)' )
      gradient.addColorStop( 1, 'rgba(255,255,255,1)' )
      context.fillStyle = gradient
      context.fillRect( 0, 0, canvas.width, canvas.height )
      var texture = new THREE.CanvasTexture( canvas )
      var geometry = new THREE.PlaneBufferGeometry( 300, 300, 3, 3 )
      var material = new THREE.MeshBasicMaterial( { map: texture, overdraw: 0.5 } )
      var mesh = new THREE.Mesh( geometry, material )
      mesh.position.y = - 250
      mesh.rotation.x = - Math.PI / 2
      group.add( mesh )
      renderer = new THREE.CanvasRenderer()
      // renderer.setPixelRatio( window.devicePixelRatio )
      THREE.WebGLRenderer()
      THREE.setPixelRatio( window.devicePixelRatio )
      renderer.setSize( window.innerWidth, window.innerHeight )
      container.appendChild( renderer.domElement )
      // stats = new Stats()
      // container.appendChild( stats.dom )
      document.addEventListener( 'mousemove', onDocumentMouseMove, false )
      //
      window.addEventListener( 'resize', onWindowResize, false )
    }
    function onWindowResize() {
      windowHalfX = window.innerWidth / 2
      windowHalfY = window.innerHeight / 2
      camera.aspect = window.innerWidth / window.innerHeight
      camera.updateProjectionMatrix()
      renderer.setSize( window.innerWidth, window.innerHeight )
    }
    function onDocumentMouseMove( event ) {
      mouseX = ( event.clientX - windowHalfX )
      mouseY = ( event.clientY - windowHalfY )
    }
    //
    function animate() {
      requestAnimationFrame( animate )
      render()
      // stats.update()
    }
    function render() {
      camera.position.x += ( mouseX - camera.position.x ) * 0.05
      camera.position.y += ( - mouseY - camera.position.y ) * 0.05
      camera.lookAt( scene.position )
      group.rotation.y -= 0.005
      renderer.render( scene, camera )
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
