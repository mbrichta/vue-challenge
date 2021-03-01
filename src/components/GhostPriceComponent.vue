<template>
  <div
    class="flex flex-col md:flex-row md:justify-between w-10/12 px-4 mb-4 bg-blue-300 p-2 rounded-lg cursor-pointer shadow-sm border border-blue-400"
    @click="$emit('new-component')"
  >
    <input class="px-2 my-1 md:my-0" placeholder="Label" disabled />
    <input class="px-2 my-1 md:my-0" placeholder="Value" disabled />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

@Component
export default class GhostPriceComponent extends Vue {}
</script>

<style scoped>
</style>
