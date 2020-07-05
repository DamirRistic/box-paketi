<template>
  <NetworkError v-if="networkErr" />
  <section class="packages" v-else-if="data">
    <div class="packages__title">
      <h1>{{ data.title }}</h1>
    </div>
    <div class="packages__contract">
      <ContractOptions
        :contracts="data.contract_length"
        @clicked="selectedOption => data.contract_length.preselected_contract_length = selectedOption"
      />
    </div>
    <div class="packages__container">
      <Packages
        v-for="(contract, index) in data.contract_length.contract_length_options"
        :key="index"
        :contract="contract"
        :activeContract="data.contract_length.preselected_contract_length"
        :packages="data.items"
        :promoText="data.promo_text"
        :images="{tv: data.assets.tv_category, net: data.assets.net_category}"
      />
    </div>
  </section>
  <Loader v-else />
</template>

<script>
import ContractOptions from "@/components/FormElements/ContractOptions";
import Packages from "@/components/UIElements/Packages";
import Loader from "@/components/UIElements/Loader";
import NetworkError from "@/components/UIElements/NetworkError";
import axios from "axios";

export default {
  name: "BoxPackages",
  components: { ContractOptions, Packages, Loader, NetworkError },
  data() {
    return {
      data: undefined,
      networkErr: false
    };
  },
  created() {
    // Fetch-ujemo podatke sa API-ja i ukoliko request bude uspesan upisujemo ih u data state u suprotnom prikazujemo NETWORK ERROR
    (async () => {
      try {
        const res = await axios.get(
          "http://www.mocky.io/v2/5ed511c53300005f00f7a790"
        );
        this.data = res.data;
      } catch (error) {
        this.networkErr = true;
      }
    })();
  }
};
</script>
