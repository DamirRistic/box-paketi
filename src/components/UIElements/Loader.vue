<template>
  <div class="loader">
    <div class="loader__wraper"></div>
    <span>LOADING</span>
  </div>
</template>

<script>
export default {
  name: "Loader"
};
</script>

<style lang="scss" scoped>
.loader {
  height: 100vh;
  width: 100vw;
  display: flex;
  flex-flow: column;
  justify-content: center;
  align-items: center;

  &__wraper {
    height: 100px;
    overflow-x: hidden;
    width: 50vw;
    position: relative;
    &:before {
      animation: loader 1s alternate infinite linear;
      background-image: linear-gradient(
        to right,
        transparent,
        $dark-color,
        transparent
      );
      content: "";
      height: 80px;
      left: 0;
      overflow: hidden;
      position: absolute;
      right: 0;
      top: 0;
    }
    &:after {
      background-image: repeating-linear-gradient(
        90deg,
        white,
        white 1vw,
        transparent 1vw,
        transparent 10vw
      );
      content: "";
      height: 80px;
      left: 0;
      overflow: hidden;
      position: absolute;
      right: 0;
      top: 0;
    }
  }

  span {
    text-align: center;
    color: #000;
    font-family: $main-font;
    font-size: 3rem;
  }
}

@keyframes loader {
  0% {
    transform: translateX(-100%);
  }
  100% {
    transform: translateX(100%);
  }
}
</style>