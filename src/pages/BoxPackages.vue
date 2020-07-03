<template>
  <section class="packages" v-if="data">
    <div class="packages__title">
      <h1>{{ data.title }}</h1>
    </div>
    <div class="packages__contract">
      <ContractOptions :contracts="data.contract_length" />
    </div>
    <div class="packages__container">
      <div class="packages__wraper"></div>
    </div>
  </section>
</template>

<script>
import ContractOptions from "@/components/ContractOptions";
import axios from "axios";

export default {
  name: "BoxPackages",
  components: { ContractOptions },
  data() {
    return {
      data: undefined
    };
  },
  created() {
    (async () => {
      const res = await axios.get(
        "http://www.mocky.io/v2/5ed511c53300005f00f7a790"
      );
      this.data = res.data;
    })();
  }
};
</script>
