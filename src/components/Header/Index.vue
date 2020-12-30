<template>
  <div class="header">
    <CellBrush v-model="pointWidth" />
    <CellColor v-model="color" />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

@Component({
  components: {
    CellBrush: () => import("./CellBrush.vue"),
    CellColor: () => import("./CellColor.vue"),
  },
})
export default class Color extends Vue {
  private color = "#284161" as string;
  private pointWidth = 5 as number;
}
</script>

<style lang="scss" scoped>
.header {
  margin: 20px;
  background: $gray-dark;
}
</style>
