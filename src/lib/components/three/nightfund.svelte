<script>
import { onMount, onDestroy } from 'svelte';
import * as THREE from "three";
import * as BufferGeometryUtils from 'three/examples/jsm/utils/BufferGeometryUtils.js'
import { darkMode } from '$lib/store/store';
// import * as dat from "lil-gui";
import gsap from "gsap";

/**
 * Debug
 */
// const gui = new dat.GUI();
 
let white = new THREE.Color(0xf0f0f0)
let pink = new THREE.Color(0xfaeded);
let black = new THREE.Color(0x232323);

const parameters = {
  materialColor: "#ffeded",
};

// gui.addColor(parameters, "materialColor").onChange(() => {
//   material.color.set(parameters.materialColor);
//   particlesMaterial.color.set(parameters.materialColor);
// });

/**
 * Base
 */
// Canvas
let container, id, sizes;
onMount(() => {
  container.appendChild(renderer.domElement);
});
onDestroy(() => cancelAnimationFrame(id));

// Scene
const scene = new THREE.Scene();


/**
 * Objects
 */
// Texture
const textureLoader = new THREE.TextureLoader();
const gradientTexture = textureLoader.load("textures/gradients/3.jpg");
gradientTexture.magFilter = THREE.NearestFilter;

// Material
const material = new THREE.MeshToonMaterial({
  color: parameters.materialColor,
  gradientMap: gradientTexture,
});

const material_alt = new THREE.MeshToonMaterial({
  color: parameters.materialColor,
  gradientMap: gradientTexture,
  wireframe: true
});

// const topBlock = new THREE.Mesh(new THREE.BoxGeometry(3, .75, .75), material_alt);
// const bottomBlock = new THREE.Mesh(new THREE.BoxGeometry(3, .75, .75), material_alt);
// const leftBlock = new THREE.Mesh(new THREE.BoxGeometry(.75, 3, .75), material_alt);
// const rightBlock = new THREE.Mesh(new THREE.BoxGeometry(.75, 3, .75), material_alt);

// topBlock.position.y = 1.125;
// bottomBlock.position.y = -1.125;
// leftBlock.position.x = - 1.125;
// rightBlock.position.x = 1.125;

// const mesh1 = new THREE.Group();
// mesh1.add(topBlock, bottomBlock, leftBlock, rightBlock);
// mesh1.scale.set(.75,.75,1)

// Objects
const objectsDistance = 4;
const mesh1 = new THREE.Mesh(new THREE.TorusGeometry(1, .5, 16, 60), material_alt);
// const mesh2 = new THREE.Mesh(new THREE.ConeGeometry(1, 2, 32), material);
const mesh3 = new THREE.Mesh(
  new THREE.TorusKnotGeometry(0.8, 0.35, 100, 16),
  material_alt
);

const size = 150;
const divisions = 50;
const mesh2 = new THREE.GridHelper( size, divisions );
mesh2.material.color = $darkMode ? pink : black;

mesh1.position.x = 0;
mesh2.position.x = 0;
mesh3.position.x = 0;

mesh1.position.y = -objectsDistance * 0;
mesh2.position.y = -objectsDistance * 1 - 0.75;
mesh3.position.y = -objectsDistance * 2;

scene.add(mesh1, mesh2, mesh3);

const sectionMeshes = [mesh1, mesh2, mesh3];

// fog
{
  const color = 0x232323;
  const density = 0.05;
  scene.fog = new THREE.FogExp2(color, density);
}

/**
 * Lights
 */
const directionalLight = new THREE.DirectionalLight("#ffffff", 1);
directionalLight.position.set(1, 1, 0);
scene.add(directionalLight);

/**
 * Particles
 */
// Geometry
const particlesCount = 200;
const positions = new Float32Array(particlesCount * 3);

for (let i = 0; i < particlesCount; i++) {
  positions[i * 3 + 0] = (Math.random() - 0.5) * 10;
  positions[i * 3 + 1] =
    objectsDistance * 0.5 -
    Math.random() * objectsDistance * sectionMeshes.length;
  positions[i * 3 + 2] = (Math.random() - 0.5) * 10;
}

const particlesGeometry = new THREE.BufferGeometry();
particlesGeometry.setAttribute(
  "position",
  new THREE.BufferAttribute(positions, 3)
);

// Material
const particlesMaterial = new THREE.PointsMaterial({
  sizeAttenuation: textureLoader,
  size: 0.03,
});

$: particlesMaterial.color = $darkMode ? pink : black;

// Points
const particles = new THREE.Points(particlesGeometry, particlesMaterial);
scene.add(particles);

/**
 * Sizes
 */
sizes = {
  width: window.innerWidth,
  height: window.innerHeight,
};

window.addEventListener("resize", () => {
  // Update sizes
  sizes.width = window.innerWidth;
  sizes.height = window.innerHeight;

  // Update camera
  camera.aspect = sizes.width / sizes.height;
  camera.updateProjectionMatrix();

  // Update renderer
  renderer.setSize(sizes.width, sizes.height);
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
});

/**
 * Camera
 */
// Group
const cameraGroup = new THREE.Group();
scene.add(cameraGroup);

// Base camera
const camera = new THREE.PerspectiveCamera(
  35,
  sizes.width / sizes.height,
  0.1,
  100
);
camera.position.z = 6;
cameraGroup.add(camera);

/**
 * Renderer
 */
const renderer = new THREE.WebGLRenderer({
  scene: scene,
  alpha: true,
});
renderer.setSize(sizes.width, sizes.height);
renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));

let scrollY = window.scrollY;
let currentSection = 0;

window.addEventListener("scroll", () => {
  scrollY = window.scrollY;
  const newSection = Math.round(scrollY / sizes.height);

  if (newSection != currentSection) {
    currentSection = newSection;

    if (currentSection != 1) {

    gsap.to(sectionMeshes[currentSection].rotation, {
      duration: 1.5,
      ease: "power2.inOut",
      x: "+=6",
      y: "+=3",
      z: "+=1.5",
    });
          
  }
  }
});

/**
 * Cursor
 */
const cursor = {};
cursor.x = 0;
cursor.y = 0;

window.addEventListener("mousemove", (event) => {
  cursor.x = event.clientX / sizes.width - 0.5;
  cursor.y = event.clientY / sizes.height - 0.5;
});

/**
 * Animate
 */
const clock = new THREE.Clock();
let previousTime = 0;

const tick = () => {
  const elapsedTime = clock.getElapsedTime();
  const deltaTime = elapsedTime - previousTime;
  previousTime = elapsedTime;

  // Animate camera
  camera.position.y = (-scrollY / sizes.height) * objectsDistance;

  const parallaxX = cursor.x * 0.5;
  const parallaxY = -cursor.y * 0.5;
  cameraGroup.position.x +=
    (parallaxX - cameraGroup.position.x) * 5 * deltaTime;
  cameraGroup.position.y +=
    (parallaxY - cameraGroup.position.y) * 5 * deltaTime;

  // Animate meshes
  let i = 0;
  for (const mesh of sectionMeshes) {
    if (i != 1) {
      mesh.rotation.x += deltaTime * 0.12;
    }
    mesh.rotation.y += deltaTime * 0.12;
    i++;
  }

  // Render
  renderer.render(scene, camera);
  // effect.render(scene, camera);

  // Call tick again on the next frame
  id = window.requestAnimationFrame(tick);
};

tick();
</script>

<div bind:this={container} class:geometry={true} />

<style>
	.geometry {
		position: fixed;
		top: 0;
		left: 0;
		overflow: hidden;
	}
</style>

