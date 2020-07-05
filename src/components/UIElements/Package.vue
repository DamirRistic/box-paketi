<template>
  <!-- Ukoliko paket opcija is_featured je true dajemo div-u klasu 'package--featured' koja regulise margin-top paketa, jer ako paket ne sadrzi div za promociju moramo preko margin-top-a da regulisemo jednakost paketa -->
  <div class="package" :class="{ 'package--featured': packageItem.is_featured }">
    <div class="package__wraper">
      <!-- Ukoliko paket opcija is_featured je true prikazujemo div za promociju -->
      <div class="package__featured" v-if="packageItem.is_featured">
        <p>{{ promoText }}</p>
      </div>
      <!-- Div koji sadrzi ime paketa, preko :style regulisemo da div bude iste visine kao i div istog tipa sa najvise sadrzaja -->
      <div class="package__name">
        <div
          class="package__name__wraper"
          ref="name"
          :style="sectionsHeights.nameHeight > 0 && { height: `${sectionsHeights.nameHeight}px`}"
        >
          <h3>{{ packageItem.name }}</h3>
        </div>
      </div>
      <!-- Div koji sadrzi tv dodatke, preko :style regulisemo da div bude iste visine kao i div istog tipa sa najvise sadrzaja -->
      <div
        class="package__tv"
        ref="tv"
        :style="sectionsHeights.tvHeight > 0 && { height: `${sectionsHeights.tvHeight}px`}"
      >
        <div class="package__img">
          <img :src="images.tv" />
        </div>
        <ul class="package__list">
          <!-- Listamo sve tv dodatke -->
          <li
            class="package__item"
            v-for="product in products.tv[packageItem.name]"
            :key="product.id"
            v-html="product.name"
          ></li>
        </ul>
      </div>
      <!-- Div koji sadrzi internet dodatke, preko :style regulisemo da div bude iste visine kao i div istog tipa sa najvise sadrzaja -->
      <div
        class="package__internet"
        ref="internet"
        :style="sectionsHeights.internetHeight > 0 && { height: `${sectionsHeights.internetHeight}px`}"
      >
        <div class="package__img">
          <img :src="images.net" />
        </div>
        <ul class="package__list">
          <!-- Listamo sve internet dodatke -->
          <li
            class="package__product"
            v-for="product in products.net[packageItem.name]"
            :key="product.id"
            v-html="product.name"
          ></li>
        </ul>
      </div>
      <div class="package__promotions">
        <!-- Listamo sve promocije -->
        <div
          class="package__promotion"
          v-for="promotion in promotions[packageItem.name]"
          :key="promotion.id"
        >
          <div class="package__promo-img">
            <img :src="promotion.img" alt="slika" />
          </div>
          <div class="package__promo-text" v-html="promotion.text"></div>
        </div>
      </div>
      <!-- Div za prikaz cene paketa -->
      <div class="package__price">
        <!-- Ukoliko paket sadrzi staru cenu prikazujemo je -->
        <div class="package__old-price" v-if="!!prices[packageItem.name].oldPrice">
          <span>{{prices[packageItem.name].oldPrice}}</span>
        </div>
        <!-- Div za prikaz nove cene -->
        <!-- Ukoliko paket ne sadrzi staru cenu onda novoj ceni dajemo :style full sirinu i veci font-size  -->
        <div
          class="package__new-price"
          :style="[!(!!prices[packageItem.name].oldPrice) && {'font-size': '2.25rem', width: '100%'}]"
        >
          <span>{{prices[packageItem.name].newPrice}}</span>
        </div>
        <!-- Div za prikaz teksta ukoliko paket sadrzi staru cenu -->
        <div
          class="package__old-price-text"
          v-if="!!prices[packageItem.name].oldPrice"
          v-html="prices[packageItem.name].text"
        ></div>
      </div>
      <!-- Dugme za narucivanje paketa -->
      <div class="package__button">
        <FormButton>Naručite</FormButton>
      </div>
    </div>
  </div>
</template>

<script>
import FormButton from "@/components/FormElements/FormButton";
export default {
  name: "Package",
  components: { FormButton },
  props: {
    packageItem: {
      type: Object,
      required: true
    },
    promoText: {
      type: String
    },
    images: {
      type: Object,
      required: true
    },
    products: {
      type: Object,
      required: true
    },
    promotions: {
      type: Object,
      required: true
    },
    prices: {
      type: Object,
      required: true
    },
    sectionsHeights: {
      type: Object,
      required: true
    }
  },
  mounted() {
    // Kada se komponenta mount-uje saljemo Parent komponenti podatke o visinama div-ova za ime, tv i internet kako bi odredili najvece visine div-ova istih tipova
    const { name, tv, internet } = this.$refs;
    this.$emit("loaded", {
      nameH: name.clientHeight,
      tvH: tv.clientHeight,
      internetH: internet.clientHeight
    });
  }
};
</script>
