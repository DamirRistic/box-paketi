<template>
  <section class="packages" v-if="data">
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
      <div
        class="packages__wraper"
        :class="data.contract_length.preselected_contract_length === contract ? 'packages__wraper--active' : 'packages__wraper--inactive'"
        v-for="(contract, index) in data.contract_length.contract_length_options"
        :key="index"
      >
        <div
          class="package"
          :class="{ 'package--featured': packet.is_featured }"
          v-for="packet in data.items"
          :key="packet.id"
        >
          <div class="package__wraper">
            <div class="package__featured" v-if="packet.is_featured">
              <p>{{ data.promo_text }}</p>
            </div>
            <div class="package__name">
              <div class="package__name__wraper">
                <h3>{{ packet.name }}</h3>
              </div>
            </div>
            <div class="package__tv">
              <div class="package__img">
                <img :src="data.assets.tv_category" />
              </div>
              <ul class="package__list">
                <li
                  class="package__item"
                  v-for="item in products.tv[packet.name]"
                  :key="item.id"
                  v-html="item.name"
                ></li>
              </ul>
            </div>
            <div class="package__internet">
              <div class="package__img">
                <img :src="data.assets.net_category" />
              </div>
              <ul class="package__list">
                <li
                  class="package__item"
                  v-for="item in products.net[packet.name]"
                  :key="item.id"
                  v-html="item.name"
                ></li>
              </ul>
            </div>
            <div class="package__promotions">
              <div
                class="package__promotion"
                v-for="promotion in promotions[packet.name][contract]"
                :key="promotion.id"
              >
                <div class="package__promo-img">
                  <img :src="promotion.img" alt="slika" />
                </div>
                <div class="package__promo-text" v-html="promotion.text"></div>
              </div>
            </div>
            <div class="package__price">
              <div class="package__old-price" v-if="!!prices[packet.name][contract].oldPrice">
                <span>{{prices[packet.name][contract].oldPrice}}</span>
              </div>
              <div
                class="package__new-price"
                :style="[!(!!prices[packet.name][contract].oldPrice) && {'font-size': '2.25rem', width: '100%'}]"
              >
                <span>{{prices[packet.name][contract].newPrice}}</span>
              </div>
              <div
                class="package__old-price-text"
                v-if="!!prices[packet.name][contract].oldPrice"
                v-html="prices[packet.name][contract].text"
              ></div>
            </div>
            <div class="package__button">
              <FormButton>Naručite</FormButton>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import ContractOptions from "@/components/FormElements/ContractOptions";
import FormButton from "@/components/FormElements/FormButton";
import axios from "axios";

export default {
  name: "BoxPackages",
  components: { ContractOptions, FormButton },
  computed: {
    products() {
      const products = this.data.items.reduce(
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
    promotions() {
      const promotions = this.data.items.reduce((acc, item) => {
        this.data.contract_length.contract_length_options.forEach(contract => {
          let promotionsList = item.promotions.filter(promotion =>
            promotion.discount_variations.includes(contract)
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
          acc[item.name] = acc[item.name]
            ? { ...acc[item.name], [contract]: promotionsList }
            : { [contract]: promotionsList };
        });
        return acc;
      }, {});
      return promotions;
    },
    prices() {
      const prices = this.data.items.reduce((acc, item) => {
        this.data.contract_length.contract_length_options.forEach(contract => {
          const {
            old_price_promo_text: text,
            old_price_recurring: oldP,
            price_recurring: newP
          } = item.prices;
          let oldPrice = this.formatPrice(oldP[contract]);
          let newPrice = oldP[contract]
            ? this.formatPrice(newP[contract])
            : this.formatPrice(newP[contract], false);
          acc[item.name] = acc[item.name]
            ? {
                ...acc[item.name],
                [contract]: { text, oldPrice, newPrice }
              }
            : { [contract]: { text, oldPrice, newPrice } };
        });
        return acc;
      }, {});

      return prices;
    }
  },
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
  },
  methods: {
    boldProduct: (string, boldedSubstring) => {
      const index = string.indexOf(boldedSubstring);
      return (
        string.substring(0, index) +
        string.substring(index, index + boldedSubstring.length).bold() +
        string.substring(index + boldedSubstring.length)
      );
    },
    formatPrice: (price, toFloat = true) => {
      if (!price) return price;
      let formatedPrice = price;
      const priceToFloat = num => {
        if (num >= 1000) {
          num /= 1000;
          num = parseFloat(num.toFixed(3));
          priceToFloat(num);
        }
        return num;
      };
      formatedPrice = parseFloat(formatedPrice).toFixed(0);
      formatedPrice = toFloat
        ? priceToFloat(parseInt(formatedPrice))
        : formatedPrice;
      return formatedPrice + " rsd/mes.";
    }
  }
};
</script>
