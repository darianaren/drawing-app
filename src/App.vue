<template>
  <h1>Drawing App 🖌️</h1>
  <section class="drawing-container">
    <nav class="toolbar">
      <input
        min="1"
        max="20"
        type="range"
        v-model="brushSize"
        @input="changeSize"
        aria-label="Change brush size"
      />
      <input
        type="color"
        v-model="brushColor"
        @input="changeColor"
        aria-label="Change brush color"
      />
      <button @click="clearCanvas" class="clear-btn">Clear</button>
      <button @click="saveDrawing" class="save-btn">Save</button>
    </nav>
    <div class="canvas-container">
      <canvas
        ref="drawingCanvas"
        class="drawing-canvas"
        @mousemove="draw"
        @mouseup="stopDrawing"
        @mousedown="startDrawing"
        @mouseleave="stopDrawing"
      >
      </canvas>
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      brushSize: 5,
      context: null,
      isDrawing: false,
      brushColor: "#000000"
    };
  },
  mounted() {
    const canvas = this.$refs.drawingCanvas;
    canvas.width = 1090;
    canvas.height = window.innerHeight * 0.55;

    this.context = canvas.getContext("2d");
    this.context.strokeStyle = this.brushColor;
    this.context.lineWidth = this.brushSize;
  },
  methods: {
    startDrawing(event) {
      this.isDrawing = true;
      this.context.beginPath();
      this.context.moveTo(event.offsetX, event.offsetY);
    },
    draw(event) {
      if (!this.isDrawing) return;
      this.context.lineTo(event.offsetX, event.offsetY);
      this.context.stroke();
    },
    stopDrawing() {
      this.isDrawing = false;
      this.context.closePath();
    },
    changeColor() {
      this.context.strokeStyle = this.brushColor;
    },
    changeSize() {
      this.context.lineWidth = this.brushSize;
    },
    clearCanvas() {
      const canvas = this.$refs.drawingCanvas;
      this.context.clearRect(0, 0, canvas.width, canvas.height);
    },
    saveDrawing() {
      const canvas = this.$refs.drawingCanvas;
      const dataURL = canvas.toDataURL("image/png");
      const link = document.createElement("a");
      link.href = dataURL;
      link.download = "drawing.png";
      link.click();
    }
  }
};
</script>

<style>
.drawing-container {
  padding: 1rem;
  max-width: 70rem;
  width: calc(100vw - 2rem);

  margin: 0 auto;
  border-radius: 1rem;
  background-color: #2e383b;
}

.toolbar {
  gap: 1rem;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: flex-end;

  width: 100%;
  margin-bottom: 1rem;
}

.canvas-container {
  width: 100%;
  height: 55vh;

  overflow: hidden;
}

.drawing-canvas {
  cursor: crosshair;
  border-radius: 0.25rem;
  background-color: #f7f4f0;
  background-image: radial-gradient(circle, #c1c6ca 1px, transparent 0);
  background-size: 40px 40px;
}

.clear-btn:hover {
  color: #f9f9f9;
  background-color: #eb552f;
}

.save-btn:hover {
  color: #f9f9f9;
  background-color: #2cc06e;
}
</style>
