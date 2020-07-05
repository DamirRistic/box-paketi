<template>
  <div class="netword-error">
    <div class="netword-error__container">
      <div class="netword-error__wraper">
        <h1>Oops!</h1>
        <h2>Network Error</h2>
      </div>
      <button @click.prevent="refreshPage">Molimo Pokusajte Kasnije</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "NetworkError",
  methods: {
    refreshPage: () => {
      window.location.reload();
    }
  }
};
</script>

<style lang="scss" scoped>
#notfound {
  position: relative;
  height: 100vh;
}

.netword-error {
  &__container {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    max-width: 520px;
    width: 100%;
    line-height: 1.4;
    text-align: center;

    button {
      font-family: "Montserrat", sans-serif;
      display: inline-block;
      font-weight: 700;
      color: #fff;
      text-transform: uppercase;
      padding: 13px 23px;
      background: #ff6300;
      font-size: 18px;
      transition: 0.2s all;
      border: none;
      cursor: pointer;

      &:hover {
        color: #ff6300;
        background: #211b19;
      }

      &:focus {
        outline: none;
      }
    }
  }

  &__wraper {
    position: relative;
    height: 200px;
    margin: 0px auto 20px;
    z-index: -1;

    h1 {
      font-family: "Montserrat", sans-serif;
      font-size: 236px;
      font-weight: 200;
      margin: 0px;
      color: #211b19;
      text-transform: uppercase;
      position: absolute;
      left: 50%;
      top: 50%;
      -webkit-transform: translate(-50%, -50%);
      -ms-transform: translate(-50%, -50%);
      transform: translate(-50%, -50%);
    }

    h2 {
      font-family: "Montserrat", sans-serif;
      font-size: 28px;
      font-weight: 400;
      text-transform: uppercase;
      color: #211b19;
      background: #fff;
      padding: 10px 5px;
      margin: auto;
      display: inline-block;
      position: absolute;
      bottom: 0px;
      left: 0;
      right: 0;
    }
  }
}

@media only screen and (max-width: 767px) {
  .netword-error__wraper h1 {
    font-size: 148px;
  }
}

@media only screen and (max-width: 480px) {
  .netword-error__wraper {
    height: 148px;
    margin: 0px auto 10px;
  }
  .netword-error__wraper h1 {
    font-size: 86px;
  }
  .netword-error__wraper h2 {
    font-size: 16px;
  }
  .netword-error__container button {
    padding: 7px 15px;
    font-size: 14px;
  }
}
</style>