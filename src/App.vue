<template>
  <h1>Drawing App</h1>
  <div class="drawing-container">
    <nav class="toolbar">
      <input
        min="1"
        max="20"
        type="range"
        v-model="brushSize"
        @input="changeSize"
      />
      <input type="color" v-model="brushColor" @input="changeColor" />
      <button @click="clearCanvas" class="clear-btn">Clear</button>
      <button @click="saveDrawing" class="save-btn">Save</button>
    </nav>
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
    canvas.width = window.innerWidth * 0.8;
    canvas.height = window.innerHeight * 0.6;

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

  border-radius: 0.5rem;
  background-color: #353264;
}

.toolbar {
  gap: 1rem;
  display: flex;
  align-items: center;
  justify-content: flex-end;

  width: 100%;
  margin-bottom: 1rem;
}

.drawing-canvas {
  cursor: crosshair;
  border-radius: 0.5rem;
  background-color: #f3f0ec;
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
