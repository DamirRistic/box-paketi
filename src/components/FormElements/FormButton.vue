<template>
  <div class="button" @click.prevent="clickHandler" tabindex="0">
    <a href="#">
      <span></span>
      <span></span>
      <span></span>
      <span></span>
      <div class="text-holder">
        <slot></slot>
      </div>
    </a>
  </div>
</template>

<script>
export default {
  name: "FormButton",
  methods: {
    clickHandler() {
      this.$emit("click");
    }
  }
};
</script>

<style scoped lang="scss">
.button {
  position: relative;
  width: 100%;
  height: 100%;
  background-color: $button-color;
  color: #fff;
  border: none;
  border-radius: 0.7rem;
  cursor: pointer;

  &:focus {
    outline: none;
  }

  a {
    width: 100%;
    height: 100%;
    display: inline-block;
    position: relative;
    color: #fff;
    text-decoration: none;
    border-radius: 0.4rem;
    overflow: hidden;
    transition: 0.2s;

    &:hover {
      color: black;
      background-color: #ae44a2;
      box-shadow: 0 0 10px #ae44a2, 0 0 20px #ae44a2;
      transition-delay: 0.4s;

      span:nth-child(1) {
        left: 0;
        transition: left 0.1s linear;
        animation: spiner 0.1s 0.4s forwards linear;
      }
      span:nth-child(2) {
        top: 0;
        transition: top 0.1s 0.1s linear;
        animation: spiner 0.1s 0.4s forwards linear;
      }
      span:nth-child(3) {
        right: 0;
        transition: right 0.1s 0.2s linear;
        animation: spiner 0.1s 0.4s forwards linear;
      }
      span:nth-child(4) {
        bottom: 0;
        transition: bottom 0.1s 0.3s linear;
        animation: spiner 0.1s 0.4s forwards linear;
      }
    }

    span {
      position: absolute;
      display: block;

      &:nth-child(1) {
        top: 2px;
        left: -100%;
        height: 6px;
        width: 100%;
        background: linear-gradient(90deg, transparent, #ae44a2);
      }
      &:nth-child(2) {
        top: -100%;
        right: 2px;
        height: 100%;
        width: 6px;
        background: linear-gradient(180deg, transparent, #ae44a2);
      }
      &:nth-child(3) {
        bottom: 2px;
        right: -100%;
        height: 6px;
        width: 100%;
        background: linear-gradient(270deg, transparent, #ae44a2);
      }
      &:nth-child(4) {
        bottom: -100%;
        left: 2px;
        height: 100%;
        width: 6px;
        background: linear-gradient(360deg, transparent, #ae44a2);
      }
    }

    .text-holder {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      font-size: 1.1rem;
      font-family: CalibriBold, sans-serif;
      color: #fff;
      z-index: 1;
    }
  }
}

@keyframes spiner {
  from {
    background: linear-gradient(360deg, transparent, #742d6c);
  }
  to {
    background: transparent;
  }
}
</style>