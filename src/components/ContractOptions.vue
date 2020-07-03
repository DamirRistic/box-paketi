<template>
  <div class="contract">
    <select v-model="contracts.preselected_contract_length">
      <option
        v-for="(contract, index) in contracts
              .contract_length_options"
        :key="index"
        :value="contract"
      >{{ contract }}</option>
    </select>
  </div>
</template>

<script>
export default {
  name: "ContractOptions",
  props: {
    contracts: {
      type: Object,
      required: true
    }
  }
};
</script>

<style scoped lang="scss">
</style>
