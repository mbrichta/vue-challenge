<template>
  <div class="flex justify-center mt-14">
    <MaterialComponent materialName="Material price Components" />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import MaterialComponent from "./components/MaterialComponent.vue";
import VTooltip from "v-tooltip";

Vue.use(VTooltip);

@Component({
  components: {
    MaterialComponent,
  },
})
export default class App extends Vue {}
</script>

<style>
</style>
