# WebGL - 3D Graphics in the Browser

## Definition

WebGL (Web Graphics Library) is a JavaScript API for rendering 3D and 2D graphics within any compatible web browser without plug-ins. It's based on OpenGL ES and uses the HTML5 canvas element.

```javascript
const gl = canvas.getContext('webgl');
```

## Basic Setup

```javascript
const canvas = document.querySelector('#glCanvas');
const gl = canvas.getContext('webgl');

if (!gl) {
  console.error('WebGL not supported');
}

// Set viewport
gl.viewport(0, 0, canvas.width, canvas.height);

// Set clear color (black)
gl.clearColor(0.0, 0.0, 0.0, 1.0);

// Clear the screen
gl.clear(gl.COLOR_BUFFER_BIT);
```

## Core Concepts

### 1. Shaders

```javascript
// Vertex Shader - positions vertices
const vertexShaderSource = `
  attribute vec4 a_position;
  void main() {
    gl_Position = a_position;
  }
`;

// Fragment Shader - colors pixels
const fragmentShaderSource = `
  precision mediump float;
  uniform vec4 u_color;
  void main() {
    gl_FragColor = u_color;
  }
`;

function createShader(gl, type, source) {
  const shader = gl.createShader(type);
  gl.shaderSource(shader, source);
  gl.compileShader(shader);
  
  if (!gl.getShaderParameter(shader, gl.COMPILE_STATUS)) {
    console.error(gl.getShaderInfoLog(shader));
    gl.deleteShader(shader);
    return null;
  }
  return shader;
}

function createProgram(gl, vertexShader, fragmentShader) {
  const program = gl.createProgram();
  gl.attachShader(program, vertexShader);
  gl.attachShader(program, fragmentShader);
  gl.linkProgram(program);
  
  if (!gl.getProgramParameter(program, gl.LINK_STATUS)) {
    console.error(gl.getProgramInfoLog(program));
    return null;
  }
  return program;
}
```

### 2. Drawing a Triangle

```javascript
const vertexShader = createShader(gl, gl.VERTEX_SHADER, vertexShaderSource);
const fragmentShader = createShader(gl, gl.FRAGMENT_SHADER, fragmentShaderSource);
const program = createProgram(gl, vertexShader, fragmentShader);

// Get attribute location
const positionAttributeLocation = gl.getAttribLocation(program, 'a_position');

// Create buffer
const positionBuffer = gl.createBuffer();
gl.bindBuffer(gl.ARRAY_BUFFER, positionBuffer);

// Triangle vertices
const positions = [
  0.0,  0.5,
 -0.5, -0.5,
  0.5, -0.5
];
gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(positions), gl.STATIC_DRAW);

// Draw
gl.useProgram(program);
gl.enableVertexAttribArray(positionAttributeLocation);
gl.bindBuffer(gl.ARRAY_BUFFER, positionBuffer);
gl.vertexAttribPointer(positionAttributeLocation, 2, gl.FLOAT, false, 0, 0);
gl.drawArrays(gl.TRIANGLES, 0, 3);
```

### 3. Uniforms (Dynamic Values)

```javascript
// Fragment shader with uniform
const fragmentSource = `
  precision mediump float;
  uniform vec4 u_color;
  void main() {
    gl_FragColor = u_color;
  }
`;

const colorLocation = gl.getUniformLocation(program, 'u_color');

// Set color
gl.uniform4f(colorLocation, 1.0, 0.0, 0.0, 1.0); // Red
gl.drawArrays(gl.TRIANGLES, 0, 3);

gl.uniform4f(colorLocation, 0.0, 1.0, 0.0, 1.0); // Green
gl.drawArrays(gl.TRIANGLES, 0, 3);
```

### 4. Textures

```javascript
function createTexture(gl, image) {
  const texture = gl.createTexture();
  gl.bindTexture(gl.TEXTURE_2D, texture);
  
  // Set parameters
  gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_WRAP_S, gl.CLAMP_TO_EDGE);
  gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_WRAP_T, gl.CLAMP_TO_EDGE);
  gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MIN_FILTER, gl.LINEAR);
  gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MAG_FILTER, gl.LINEAR);
  
  // Upload image
  gl.texImage2D(gl.TEXTURE_2D, 0, gl.RGBA, gl.RGBA, gl.UNSIGNED_BYTE, image);
  
  return texture;
}

// Load image
const image = new Image();
image.src = 'texture.jpg';
image.onload = () => {
  const texture = createTexture(gl, image);
  // Use texture in rendering
};
```

### 5. Matrices and Transformations

```javascript
// Simple matrix library (for demonstration)
function perspectiveMatrix(fov, aspect, near, far) {
  const f = 1.0 / Math.tan(fov / 2);
  const rangeInv = 1.0 / (near - far);
  
  return new Float32Array([
    f / aspect, 0, 0, 0,
    0, f, 0, 0,
    0, 0, (near + far) * rangeInv, -1,
    0, 0, near * far * rangeInv * 2, 0
  ]);
}

function translationMatrix(tx, ty, tz) {
  return new Float32Array([
    1, 0, 0, 0,
    0, 1, 0, 0,
    0, 0, 1, 0,
    tx, ty, tz, 1
  ]);
}

function rotationYMatrix(angle) {
  const c = Math.cos(angle);
  const s = Math.sin(angle);
  
  return new Float32Array([
    c, 0, s, 0,
    0, 1, 0, 0,
    -s, 0, c, 0,
    0, 0, 0, 1
  ]);
}
```

## Common Use Cases

### 1. 3D Cube

```javascript
const cubeVertices = new Float32Array([
  // Front face
  -1.0, -1.0,  1.0,
   1.0, -1.0,  1.0,
   1.0,  1.0,  1.0,
  -1.0,  1.0,  1.0,
  // Back face
  -1.0, -1.0, -1.0,
  -1.0,  1.0, -1.0,
   1.0,  1.0, -1.0,
   1.0, -1.0, -1.0,
  // ... more faces
]);

const cubeColors = new Float32Array([
  1.0, 0.0, 0.0, 1.0,  // Front: red
  0.0, 1.0, 0.0, 1.0,  // Back: green
  0.0, 0.0, 1.0, 1.0,  // Top: blue
  1.0, 1.0, 0.0, 1.0,  // Bottom: yellow
  1.0, 0.0, 1.0, 1.0,  // Right: magenta
  0.0, 1.0, 1.0, 1.0   // Left: cyan
]);

function drawCube(gl, program, time) {
  const angle = time * 0.001;
  
  // Set uniforms
  const modelViewMatrix = gl.getUniformLocation(program, 'u_modelViewMatrix');
  const projectionMatrix = gl.getUniformLocation(program, 'u_projectionMatrix');
  
  gl.uniformMatrix4fv(modelViewMatrix, false, 
    multiply(translationMatrix(0, 0, -6), rotationYMatrix(angle))
  );
  gl.uniformMatrix4fv(projectionMatrix, false, 
    perspectiveMatrix(Math.PI / 4, canvas.width / canvas.height, 0.1, 100)
  );
  
  // Draw each face
  for (let i = 0; i < 6; i++) {
    gl.uniform4fv(colorLocation, cubeColors.slice(i * 4, i * 4 + 4));
    gl.drawArrays(gl.TRIANGLE_FAN, i * 4, 4);
  }
}
```

### 2. Interactive Visualization

```javascript
class WebGLApp {
  constructor(canvas) {
    this.gl = canvas.getContext('webgl');
    this.program = null;
    this.buffers = {};
  }
  
  async init() {
    this.program = await this.createProgram(
      vertexShaderSource,
      fragmentShaderSource
    );
    this.setupBuffers();
    this.setupEventListeners();
    this.render();
  }
  
  setupEventListeners() {
    canvas.addEventListener('mousemove', (e) => {
      this.mouseX = e.clientX / canvas.width * 2 - 1;
      this.mouseY = -(e.clientY / canvas.height * 2 - 1);
    });
    
    canvas.addEventListener('click', () => {
      this.clicked = !this.clicked;
    });
  }
  
  render() {
    const gl = this.gl;
    gl.clear(gl.COLOR_BUFFER_BIT | gl.DEPTH_BUFFER_BIT);
    
    // Use mouse position for interactive effects
    gl.uniform2f(mouseLocation, this.mouseX, this.mouseY);
    gl.uniform1f(timeLocation, performance.now() * 0.001);
    
    // Draw shapes
    this.drawScene();
    
    requestAnimationFrame(() => this.render());
  }
}

const app = new WebGLApp(canvas);
app.init();
```

## Common Mistakes

```javascript
// ❌ Wrong: Not checking for WebGL support
const gl = canvas.getContext('webgl');

// ✅ Correct: Always check
const gl = canvas.getContext('webgl');
if (!gl) {
  console.error('WebGL not supported');
  return;
}

// ❌ Wrong: Using wrong buffer type
gl.bufferData(gl.ARRAY_BUFFER, [1, 2, 3], gl.STATIC_DRAW);

// ✅ Correct: Use typed arrays
gl.bufferData(gl.ARRAY_BUFFER, new Float32Array([1, 2, 3]), gl.STATIC_DRAW);

// ❌ Wrong: Not linking program
const program = gl.createProgram();
gl.attachShader(program, vertexShader);
// Missing: gl.linkProgram(program);

// ✅ Correct: Complete the program setup
gl.linkProgram(program);
if (!gl.getProgramParameter(program, gl.LINK_STATUS)) {
  console.error(gl.getProgramInfoLog(program));
}
```

## Useful Libraries

```javascript
// Three.js (popular 3D library)
import * as THREE from 'three';

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, w / h, 0.1, 1000);
const renderer = new THREE.WebGLRenderer({ canvas });

const geometry = new THREE.BoxGeometry(1, 1, 1);
const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
const cube = new THREE.Mesh(geometry, material);

scene.add(cube);
camera.position.z = 5;

function animate() {
  requestAnimationFrame(animate);
  cube.rotation.x += 0.01;
  cube.rotation.y += 0.01;
  renderer.render(scene, camera);
}
animate();
```

## Quick Revision Summary

- WebGL provides low-level 3D graphics in the browser
- Use shaders (vertex + fragment) for rendering
- Buffers store vertex data
- Uniforms pass dynamic values to shaders
- Textures wrap images onto surfaces
- Use libraries like Three.js for complex applications

## Related Topics

- [[Canvas2D]] - 2D drawing API
- [[requestAnimationFrame]] - Smooth animations
- [[TypedArrays]] - Efficient data storage for WebGL
- [[Math]] - Vector and matrix operations
- [[Performance]] - Optimizing WebGL rendering
