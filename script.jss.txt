// Get the container for the canvas
const container = document.getElementById('canvas-container');

// Create Scene, Camera, Renderer
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, 1, 0.1, 1000);
const renderer = new THREE.WebGLRenderer();
renderer.setSize(400, 400);
container.appendChild(renderer.domElement);

// Add Lighting
const light = new THREE.DirectionalLight(0xffffff, 1);
light.position.set(10, 10, 10).normalize();
scene.add(light);

// Enable OrbitControls
const controls = new THREE.OrbitControls(camera, renderer.domElement);
controls.enableDamping = true; // Smooth motion
controls.dampingFactor = 0.1;
controls.rotateSpeed = 0.5;

// Create Minecraft character
const bodyGeometry = new THREE.BoxGeometry(1, 2, 1);
const headGeometry = new THREE.BoxGeometry(1, 1, 1);

// Load textures
const loader = new THREE.TextureLoader();
const steveTexture = loader.load('assets/steve.png');
const bodyMaterial = new THREE.MeshBasicMaterial({ map: steveTexture });
const headMaterial = new THREE.MeshBasicMaterial({ map: steveTexture });

// Create character parts
const body = new THREE.Mesh(bodyGeometry, bodyMaterial);
const head = new THREE.Mesh(headGeometry, headMaterial);
body.position.y = 0;
head.position.y = 1.5;
scene.add(body, head);

// Set Camera Position
camera.position.z = 5;

// Armor textures
const armorTextures = {
  helmet: {
    diamond: loader.load('assets/diamond_helmet.png'),
    netherite: loader.load('assets/netherite_helmet.png'),
    gold: loader.load('assets/gold_helmet.png'),
    iron: loader.load('assets/iron_helmet.png'),
  },
  chestplate: {
    diamond: loader.load('assets/diamond_chestplate.png'),
    netherite: loader.load('assets/netherite_chestplate.png'),
    gold: loader.load('assets/gold_chestplate.png'),
    iron: loader.load('assets/iron_chestplate.png'),
  },
  leggings: {
    diamond: loader.load('assets/diamond_leggings.png'),
    netherite: loader.load('assets/netherite_leggings.png'),
    gold: loader.load('assets/gold_leggings.png'),
    iron: loader.load('assets/iron_leggings.png'),
  },
  boots: {
    diamond: loader.load('assets/diamond_boots.png'),
    netherite: loader.load('assets/netherite_boots.png'),
    gold: loader.load('assets/gold_boots.png'),
    iron: loader.load('assets/iron_boots.png'),
  },
};

// Apply individual armor parts
const armorMaterials = { helmet: headMaterial, chestplate: bodyMaterial, leggings: bodyMaterial, boots: bodyMaterial };

function applyArmorPart(part, type) {
  const texture = armorTextures[part][type];
  const material = new THREE.MeshBasicMaterial({ map: texture, transparent: true });
  armorMaterials[part] = material;

  // Update materials
  if (part === 'helmet') head.material = material;
  else body.material = material;
}

// Animation loop
function animate() {
  requestAnimationFrame(animate);
  controls.update(); // Update OrbitControls
  renderer.render(scene, camera);
}

animate();
