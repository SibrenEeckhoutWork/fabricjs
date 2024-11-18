<template>
  <div class="center">
    <button @click="addRectangle">Add Rectangle</button>
    <canvas id="canvas" ref="canvas"></canvas>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import * as fabric from 'fabric';

// Initialize the canvas reference as null in the setup function
let canvas = ref(null);

// Canvas instance
let fabricCanvas = null;

onMounted(() => {
  // Initialize fabric canvas when the component is mounted
  fabricCanvas = new fabric.Canvas('canvas', {
    width: 1500,
    height: 800,
    selection: true, // Enable object selection
  });
  // Assign the fabric canvas instance to the reactive canvas reference
  canvas.value = fabricCanvas;

  // Automatically load the canvas state from localStorage on page load
  loadCanvas();

  // Automatically save whenever an object is added, moved, or scaled
  fabricCanvas.on('object:added', saveCanvas);
  fabricCanvas.on('object:moving', saveCanvas);
  fabricCanvas.on('object:scaling', saveCanvas);
  loadCanvas();
});

// Function to add a rectangle to the canvas
const addRectangle = () => {
  const rect = new fabric.Rect({
    top: 100,
    left: 100,
    width: 100,
    height: 100,
    fill: 'rgba(0, 0, 255, 0.05)', 
    stroke: 'blue',
    strokeWidth: 2,
  });

  // Add the rectangle to the canvas and set it as the active object
  fabricCanvas.add(rect);
  fabricCanvas.setActiveObject(rect);

  // Ensure the canvas renders properly with updated controls
  fabricCanvas.renderAll();
};

// Function to save the canvas objects to localStorage
const saveCanvas = () => {
  // Get the JSON representation of the canvas
  const canvasData = fabricCanvas.toJSON();

  // Save it to localStorage
  localStorage.setItem('canvasData', JSON.stringify(canvasData));
};

// Function to load the canvas objects from localStorage
const loadCanvas = () => {
  // Get the saved canvas data from localStorage
  const savedCanvasData = localStorage.getItem('canvasData');

  if (savedCanvasData) {
    // Parse the saved data and load it into the canvas
    fabricCanvas.loadFromJSON(JSON.parse(savedCanvasData), () => {
      // After loading, render all objects immediately
      fabricCanvas.renderAll();
    });
  }
};
</script>

<style scoped>
.center {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 20px;
}

canvas {
  border: 1px solid #ccc;
  background: url('../public/plan.svg') no-repeat center center;
  background-size: cover;
}
</style>
