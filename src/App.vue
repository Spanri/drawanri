<template>
  <div id="app" class="app">
    <h1 class="app__title">Drawanri = Draw + Spanri</h1>

    <div>
      <label>Color</label>
      <input type="color" id="head" v-model="color" name="head" />
      <input v-model="color" />
    </div>

    <br />

    <div>
      <label>Width</label>
      <input type="range" min="0" max="30" step="1" v-model="pointWidth" />
      <input v-model="pointWidth" type="number" />
    </div>

    <br />

    <div>
      <canvas class="app__canvas" id="canvas" width="512" height="512"></canvas>
    </div>

    <br />

    <button @click="undoLastLine()">Undo last line</button>
    <button @click="downloadImage()">Download my masterpiece</button>
  </div>
</template>

<script lang="ts">
import { Component, Watch, Vue } from "vue-property-decorator";

@Component({ components: {} })
export default class App extends Vue {
  private ctx: any | null = null;

  private color = "#284161" as string;
  private pointWidth = 5 as number;
  private lines: { x: number; y: number; pointWidth: number }[][] = [];
  private isDrawing = false as boolean;

  @Watch("color")
  colorChanged(newColor: string) {
    this.ctx.fillStyle = newColor;
  }

  mounted(): void {
    window.addEventListener("mousedown", this.startDrawingStatus);
    window.addEventListener("mouseup", this.endDrawingStatus);

    const canvas: any | null = document.getElementById("canvas");

    if (canvas) {
      canvas.addEventListener("mousedown", this.setPointStart);
      canvas.addEventListener("mouseup", this.setPointEnd);
      canvas.addEventListener("mousemove", this.drawPoint);

      this.ctx = canvas!.getContext("2d");
      this.ctx.fillStyle = "white";
      this.ctx.fillRect(1, 1, 512, 512);
      this.ctx.fillStyle = this.color;
    }
  }

  private startDrawingStatus() {
    this.isDrawing = true;
  }

  private endDrawingStatus() {
    this.isDrawing = false;
  }

  private setPointStart(event: any) {
    this.isDrawing = true;
    this.lines.push([]);

    this.ctx.beginPath();
    this.ctx.fillRect(
      event.offsetX,
      event.offsetY,
      this.pointWidth,
      this.pointWidth
    );
  }

  private setPointEnd(event: any) {
    this.isDrawing = false;
  }

  private drawPoint(event: any) {
    if (this.isDrawing) {
      this.lines[this.lines.length - 1].push({
        x: event.offsetX,
        y: event.offsetY,
        pointWidth: this.pointWidth,
      });

      this.ctx.beginPath();
      this.ctx.fillRect(
        event.offsetX,
        event.offsetY,
        this.pointWidth,
        this.pointWidth
      );
    }
  }

  private undoLastLine() {
    this.lines.pop();
    this.redrawAll();
  }

  private redrawAll() {
    this.ctx.clearRect(0, 0, 512, 512);

    for (let i = 0; i < this.lines.length; i++) {
      const line = this.lines[i];

      for (let j = 0; j < line.length; j++) {
        const point = line[j];
        this.ctx.fillRect(point.x, point.y, point.pointWidth, point.pointWidth);
      }
    }
  }

  private downloadImage() {
    const link = document.createElement("a");
    link.download = "filename.png";
    const canvas: any = document.getElementById("canvas");
    if (canvas) {
      link.href = canvas!.toDataURL();
      link.click();
    }
  }
}
</script>

<style lang="scss">
$blue-dark: #284161;

html {
  height: 100%;
}

body {
  height: 100%;
  margin: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: $blue-dark;
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
}
</style>

<style lang="scss" scoped>
$pink: #fae3e7;
$white: white;

.app {
  background: $pink;

  &__title {
    margin: 0;
    padding: 30px;
  }

  &__canvas {
    width: 512px;
    height: 512px;
    border-radius: 7px;
    background: $white;
  }
}
</style>
