<template>
  <!-- Div koji sadrzi sve pakete za odabranu contract opciju -->
  <!-- Ukoliko se poklapa sa aktivnim contract-om iz contract opcija prikazujemo ga tako sto mu dajemo class-u 'packages__wraper--active' ukoliko se ne poklapa sklanjamo ga u stranu tako sto mu dajemo class-u 'packages__wraper--inactive' -->
  <div
    class="packages__wraper"
    :class="activeContract === contract ? 'packages__wraper--active' : 'packages__wraper--inactive'"
  >
    <!-- Listamo sve pakete za izabrani contract i prosledjujemo state -->
    <Package
      v-for="packageItem in packages"
      :key="packageItem.id"
      :packageItem="packageItem"
      :promoText="promoText"
      :images="images"
      :products="products"
      :promotions="promotions"
      :prices="prices"
      :sectionsHeights="{tvHeight, nameHeight, internetHeight}"
      @loaded="setPackageHeight"
    />
  </div>
</template>

<script>
import Package from "@/components/UIElements/Package";
export default {
  name: "Packages",
  components: { Package },
  props: {
    contract: {
      type: String,
      required: true
    },
    activeContract: {
      type: String,
      required: true
    },
    packages: {
      type: Array,
      required: true
    },
    promoText: {
      type: String,
      default: "Preporuka"
    },
    images: {
      type: Object,
      required: true
    }
  },
  computed: {
    // Od array-a sa svim internet i tv usluga pravimo object koji sadrzi odvojene tv i internet usluge za svaki paket
    products() {
      const products = this.packages.reduce(
        (acc, item) => {
          item.included.map(product => {
            if (product.product_category === "tv") {
              acc.tv[item.name] = acc.tv[item.name]
                ? [
                    ...acc.tv[item.name],
                    {
                      id: product.id,
                      name: this.boldProduct(
                        product.long_name,
                        product.attributes.attribute_value
                      )
                    }
                  ]
                : [
                    {
                      id: product.id,
                      name: this.boldProduct(
                        product.long_name,
                        product.attributes.attribute_value
                      )
                    }
                  ];
            } else {
              acc.net[item.name] = acc.net[item.name]
                ? [
                    ...acc.net[item.name],
                    {
                      id: product.id,
                      name: this.boldProduct(
                        product.long_name,
                        product.attributes.attribute_value
                      )
                    }
                  ]
                : [
                    {
                      id: product.id,
                      name: this.boldProduct(
                        product.long_name,
                        product.attributes.attribute_value
                      )
                    }
                  ];
            }
          });
          return acc;
        },
        { tv: {}, net: {} }
      );
      return products;
    },
    // Pravimo object koji sadrzi promocije koje vaze za contract koji je izabran za sve pakete
    promotions() {
      const promotions = this.packages.reduce((acc, item) => {
        let promotionsList = item.promotions.filter(promotion =>
          promotion.discount_variations.includes(this.contract)
        );
        if (promotionsList.length > 0) {
          promotionsList = promotionsList.map(promotion => {
            return {
              id: promotion.id,
              text: promotion.promo_text,
              img: promotion.promotion_image
            };
          });
        }
        acc[item.name] = promotionsList;
        return acc;
      }, {});
      return promotions;
    },
    // Pravimo object sa formatiranim cenama za svaki paket
    prices() {
      const prices = this.packages.reduce((acc, item) => {
        const {
          old_price_promo_text: text,
          old_price_recurring: oldP,
          price_recurring: newP
        } = item.prices;
        let oldPrice = this.formatPrice(oldP[this.contract]);
        let newPrice = oldP[this.contract]
          ? this.formatPrice(newP[this.contract])
          : this.formatPrice(newP[this.contract], false);
        acc[item.name] = { text, oldPrice, newPrice };
        return acc;
      }, {});

      return prices;
    }
  },
  data() {
    return {
      tvHeight: 0,
      nameHeight: 0,
      internetHeight: 0
    };
  },
  methods: {
    // Funkcija kojom bold-ujemo delove teksta za tv i internet usluge
    boldProduct: (string, boldedSubstring) => {
      const index = string.indexOf(boldedSubstring);
      return (
        string.substring(0, index) +
        string.substring(index, index + boldedSubstring.length).bold() +
        string.substring(index + boldedSubstring.length)
      );
    },
    // Funkcija kojom formatirmo cene paketa
    formatPrice: (price, toFloat = true) => {
      // Ukoliko cena ne postoji vracamo je nazad kakva jeste
      if (!price) return price;
      let formatedPrice = price;
      // Pomocna funkcija za formatiranje cene sa tackama, primer 10000000 = 10.000.000
      const priceToFloat = (num, modulo = "") => {
        if (num >= 1000) {
          let formatedModulo = (num % 1000).toString().split("");
          let formatedModuloLength = formatedModulo.length;
          for (let i = formatedModuloLength; i < 3; i++) {
            formatedModulo.unshift("0");
          }
          modulo = `.${formatedModulo.join("")}${modulo}`;
          num = (num - (num % 1000)) / 1000;
          priceToFloat(num, modulo);
        } else {
          formatedPrice = modulo ? num + modulo : num;
        }
      };
      // Izbacujemo .0000 koje ne koristimo nigde u prikazu
      formatedPrice = parseFloat(formatedPrice).toFixed(0);
      // Ukoliko smo kao drugi argument funkcije uneli true formatiramo cenu sa tackama preko funkcije priceToFloat
      toFloat && priceToFloat(parseInt(formatedPrice));
      return formatedPrice + " rsd/mes.";
    },
    // Funkcija kojoj saljemo vredntosti visina div-ova i koja odredjuje najvecu visinu div-ova istog tipa
    setPackageHeight({ nameH, tvH, internetH }) {
      this.nameHeight = nameH > this.nameHeight ? nameH : this.nameHeight;
      this.tvHeight = tvH > this.tvHeight ? tvH : this.tvHeight;
      this.internetHeight =
        internetH > this.internetHeight ? internetH : this.internetHeight;
    }
  }
};
</script>
