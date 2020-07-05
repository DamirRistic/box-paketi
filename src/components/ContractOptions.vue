<template>
  <div class="contract" @blur="showOptions = false" tabindex="0">
    <!-- Div koji simulira Select tag i sadrzi aktivni contract -->
    <!-- Na njegov click regulisemo prikaz dropdown-a sa contract opcijama -->
    <div
      class="contract__selected"
      @click="showOptions = !showOptions"
      :style="[showOptions === true ? {'border-bottom-left-radius': '0', 'border-bottom-right-radius': '0'} : {'border-bottom-left-radius': '0.7rem', 'border-bottom-right-radius': '0.7rem'}]"
    >
      <span>{{contracts.preselected_contract_length}}</span>
      <img src="../assets/images/icons/angle-bottom.svg" alt="select" />
    </div>
    <!-- Div koji sadrzi contract opcije -->
    <div class="contract__options" v-if="showOptions">
      <div
        class="contract__option"
        :class="{'contract__option--active' : contract === contracts.preselected_contract_length}"
        @click="selectHandle(contract)"
        v-for="(contract, index) in contracts
              .contract_length_options"
        :key="index"
      >{{contract}}</div>
    </div>
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
  },
  data() {
    return {
      //State za regulisanje prikaza contract dropdown opcija
      showOptions: false
    };
  },
  methods: {
    // Funkcija kojom selektujemo contract opciju i saljemo je u Parent komponentu
    selectHandle(selectedOption) {
      this.$emit("clicked", selectedOption);
      this.showOptions = false;
    }
  }
};
</script>

<style scoped lang="scss">
.contract {
  width: 181px;
  height: 41px;

  &:focus {
    outline: none;
  }

  &__selected {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    background-color: $light-color;
    color: #757472;
    border: 1px solid #d3d3d3;
    border-radius: 0.7rem;
    font-size: 0.9rem;
    padding: 0.2rem 0 0.2rem 0.7rem;
    margin-bottom: 2px;

    &:hover {
      cursor: pointer;
    }

    &:focus {
      outline: none;
      border-bottom-left-radius: 0;
      border-bottom-right-radius: 0;
    }

    img {
      height: 15px;
      margin: auto 10px auto auto;
    }
  }

  &__options {
    width: 100%;
    display: flex;
    flex-flow: column-reverse;
    margin-top: 4px;
    position: relative;
    z-index: 1;
  }

  &__option {
    width: 100%;
    height: 41px;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    background-color: $light-color;
    color: #757472;
    font-size: 0.9rem;
    padding: 0.2rem 0 0.2rem 0.7rem;
    border-right: 1px solid #d3d3d3;
    border-left: 1px solid #d3d3d3;

    &:first-child {
      border-bottom: 1px solid #d3d3d3;
      border-bottom-left-radius: 0.7rem;
      border-bottom-right-radius: 0.7rem;
    }

    &:hover {
      background-color: $dark-color;
      color: #fff;
      cursor: pointer;
    }

    &--active {
      background-color: $dark-color;
      color: #fff;
    }
  }
}
</style>
