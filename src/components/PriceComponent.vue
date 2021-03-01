<template>
  <div
    class="flex flex-col md:flex-row md:justify-between w-10/12 px-4 mb-4"
    @mouseover="hover = true"
    @mouseleave="hover = false"
  >
    <div class="flex overflow-ellipsis overflow-hidden">
      <label
        class="font-mono overflow-ellipsis mb-1 md:mb-0"
        v-if="!labelEdit"
        v-tooltip.left="{
          content: component.label,
          classes: 'bg-gray-200 px-1 rounded mr-1',
        }"
        >{{ displayLabel() }}</label
      >
      <input
        class="px-2 mb-2 md:mb-0"
        v-if="labelEdit"
        v-model="component.label"
        @keyup.enter="checkLabel(component.label)"
        @blur="checkLabel(component.label)"
        :placeholder="labelPlaceholder"
        v-tooltip.left="{
          content: component.label,
          classes: 'bg-gray-200 px-1 rounded mr-1',
        }"
      />
      <EditIcon
        v-if="hover && editable"
        v-on:click.native="labelEdit = !labelEdit"
      />
    </div>
    <div class="flex">
      <DeleteIcon
        v-if="hover && editable"
        v-on:click.native="$emit('delete-component')"
      />
      <input
        class="px-2"
        v-model="component.value"
        :placeholder="component.value"
        @focus="component.value = Number(component.value)"
        @blur="checkValue(component.value)"
        :disabled="!editable"
        @keyup.enter="checkValue(component.value)"
        v-tooltip.right="{
          content: component.value,
          classes: 'bg-gray-200 px-1 rounded ml-1',
        }"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import EditIcon from "./EditIcon.vue";
import DeleteIcon from "./DeleteIcon.vue";
import { PriceComponentType } from "../types/index";

@Component({
  components: {
    EditIcon,
    DeleteIcon,
  },
})
export default class PriceComponent extends Vue {
  @Prop({ default: {} }) component!: PriceComponentType;
  @Prop({ default: true }) editable!: boolean;

  label = "";
  value = 0;
  hover = false;
  labelEdit = false;
  labelPlaceholder = "";

  displayLabel() {
    if (this.component.label.length < 1) {
      this.labelEdit = true;
    } else {
      return this.component.label;
    }
  }

  checkValue(value: string) {
    if (isNaN(Number(value))) {
      this.component.value = "";
    } else if (Number(value) < 1) {
      this.component.value = "0.0";
    } else {
      this.component.value = value;
      this.labelEdit = false;
      this.$emit("format-value", this.component.id);
    }
  }

  checkLabel(label: string) {
    if (label.trim().length < 1) {
      this.labelPlaceholder = "Enter Name";
      this.component.label = "";
      this.labelEdit = true;
    } else {
      this.component.label = label;
      this.labelEdit = false;
    }
  }
}
</script>

<style scoped>
</style>
