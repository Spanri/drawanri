<template>
  <div class="ui-input-wrapper">
    <input
      class="ui-input"
      v-bind="$attrs"
      v-on="$listeners"
      :value="value"
      @input="$emit('change', $event.target)"
    />
  </div>
</template>

<script lang="ts">
import { Component, Prop, Emit, Model, Vue } from "vue-property-decorator";

@Component({ components: {} })
export default class UiInput extends Vue {
  @Prop() @Model("change") readonly value: number;
}
</script>

<style lang="scss" scoped>
.ui-input {
  background: white;
  color: #284161;
  padding: 7px 13px;
  font-size: 14px;
  border-radius: 4px;
  border: 0;
}
</style>
