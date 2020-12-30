<template>
  <div class="color">
    <p>Color</p>

    <Input type="color" id="head" v-model="innerColor" name="head" />
    <Input v-model="innerColor" />
  </div>
</template>

<script lang="ts">
import { Component, Prop, Emit, Model, Vue } from "vue-property-decorator";

@Component({
  components: {
    Input: () => import("../_Input.vue"),
  },
})
export default class Color extends Vue {
  @Prop() @Model("change") readonly color: number;

  get innerColor(): number {
    return this.color;
  }

  set innerColor(value: number) {
    this.$emit("change", value);
  }
}
</script>
