<template>
    <div>
      <h1>Draw Custom Shapes</h1>
  
      <!-- The canvas element -->
      <canvas ref="canvas" id="canvas" width="600" height="400"></canvas>
  
      <!-- Controls -->
      <div>
        <button @click="startDrawing">Start Drawing</button>
        <button @click="saveDrawing">Save Drawing</button>
        <button @click="clearCanvas">Clear Canvas</button>
      </div>
    </div>
  </template>
  
  <script>
    import * as fabric from 'fabric';
  
  export default {
    data() {
      return {
        canvas: null,
      };
    },
    mounted() {
      // Initialize the Fabric.js canvas when the component is mounted
      this.canvas = new fabric.Canvas(this.$refs.canvas);
  
      // Enable free drawing mode
      this.canvas.isDrawingMode = false; // Drawing starts off disabled
  
      // Set default drawing properties
      this.canvas.freeDrawingBrush.color = 'blue'; // Pen color
      this.canvas.freeDrawingBrush.width = 5;      // Pen width
    },
    methods: {
      startDrawing() {
        // Enable free drawing mode
        this.canvas.isDrawingMode = true;
      },
      saveDrawing() {
        // Save the free-drawn path as a fabric.Path
        const drawingPath = this.canvas.toJSON().objects.find((obj) => obj.type === 'path');
        if (drawingPath) {
          const path = new fabric.Path(drawingPath.path, {
            fill: 'rgba(0, 255, 0, 0.5)', // Fill color for the custom shape
            stroke: 'black',              // Stroke color
            strokeWidth: 2,               // Stroke width
          });
  
          // Add the path to the canvas
          this.canvas.add(path);
  
          // Optionally clear the drawing to make space for other shapes
          this.canvas.clear();
        }
      },
      clearCanvas() {
        // Clear the entire canvas
        this.canvas.clear();
      },
    },
  };
  </script>
  
  <style scoped>
  #canvas {
    border: 1px solid black;
  }
  
  button {
    margin: 10px;
  }
  </style>
  