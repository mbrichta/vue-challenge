<template>
  <div
    class="flex flex-col items-center bg-blue-200 p-2 md:p-4 rounded-lg w-10/12 lg:max-w-screen-xl shadow-lg"
  >
    <h1 class="text-xl font-bold font-mono mb-4">{{ materialName }}</h1>
    <div class="flex justify-end w-10/12 px-4">
      <h1 class="font-mono mb-4">Total Price: {{ calTotalPrice }} EUR/KG</h1>
    </div>
    <PriceComponent :component="basePrice" :editable="false" />
    <PriceComponent
      v-for="(component, key) in components"
      v-bind:key="key"
      :component="component"
      @delete-component="deletePriceComponent()"
      @format-value="formatValue(component.id)"
      @zero-value="deletePriceComponent"
    />
    <GhostPriceComponent @new-component="newPriceComponent(label, value)" />
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import PriceComponent from "./PriceComponent.vue";
import GhostPriceComponent from "./GhostPriceComponent.vue";
import { PriceComponentType } from "../types/index";

@Component({
  components: {
    PriceComponent,
    GhostPriceComponent,
  },
})
export default class MaterialComponent extends Vue {
  @Prop() materialName!: string;

  basePrice: PriceComponentType = { id: 0, label: "Baseprice", value: "1.00" };
  components: PriceComponentType[] = [];
  label = "";
  value = 0;
  hover = false;

  newPriceComponent(): void {
    this.components.push({
      id: this.assignId(),
      label: "",
      value: Number("0").toFixed(2),
    });
    this.label = "";
    this.value = 0;
  }

  get calTotalPrice() {
    const values = this.components.map((comp) => Number(comp.value));

    return (
      values.reduce((acc, red) => acc + red, 0) + Number(this.basePrice.value)
    ).toFixed(2);
  }

  formatValue(id: number) {
    const indxOfComponent = this.components.map((comp) => comp.id).indexOf(id);

    this.components[indxOfComponent].value = Number(
      this.components[indxOfComponent].value
    ).toFixed(2);
  }

  assignId() {
    if (this.components.length === 0) {
      return 1;
    }
    const lastItemId = this.components[this.components.length - 1].id;

    console.log(lastItemId);
    return lastItemId + 1;
  }

  deletePriceComponent(id: number) {
    const indxOfComponent = this.components.map((comp) => comp.id).indexOf(id);
    this.components.splice(indxOfComponent, 1);
  }
}
</script>

<style scoped>
</style>
