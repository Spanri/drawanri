<template>
  <div class="brush">
    <p>Brush</p>

    <div>
      <label>Width</label>
      <Input type="range" min="0" max="30" step="1" v-model="innerPointWidth" />
      <Input v-model="innerPointWidth" type="number" />
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Emit, Model, Vue } from "vue-property-decorator";

@Component({
  components: {
    Input: () => import("../_Input.vue"),
  },
})
export default class Brush extends Vue {
  @Prop() @Model("change") readonly pointWidth: number;

  get innerPointWidth(): number {
    return this.pointWidth;
  }

  set innerPointWidth(value: number) {
    this.$emit("change", value);
  }
}
</script>
