<template>
  <div>
    <div class="container-fluid py-5">
      <div class="container-fluid">
        <div class="row g-5">
          <div class="col-lg-2 col-md-3 fadeInUp">
            <CategorySidebar/>
            <div class="widget mt-5">
              <h6 class="widgettitle">
                <span class="text">Sort by</span>
              </h6>
              <div class="widget-content">
                <ul class="filter-order-by">
                  <li v-for="item in items" :key="item.id" :class="{ selected: item.isSelected }">
                    <a @click="handleClick(item)">{{ item.name }}</a>
                  </li>

                </ul>
              </div>
            </div>
            <div id="app">
              <div class='range-slider'>
                <input type="range" min="0" :max="max" step="1" v-model="sliderMin">
                <input type="range" min="0" :max="max" step="1" v-model="sliderMax">
              </div>
              <div class="price_label mt-2 text-center">
                Price: <span class="from">{{sliderMin}}€</span> — <span class="to">{{sliderMax}}€</span>
              </div>
            </div>
          </div>

          <div class="col-lg-10 col-md-9 fadeInUp">
            <Products :products="products.slice((page - 1) * 12, page * 12)" :page="page"/>
            <Pagination class="mt-5 d-flex justify-content-center text-center" :data="products" :per-page="12" :records="products.length" v-model="page"></Pagination>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import products from "~/assets/data/products.json";
import config from "~/assets/data/config.json";
import Pagination from 'vue-pagination-2';

export default {
  name: "categoryComponent",
  head() {
    return {
      title: config.seo.shop
          ? config.seo.shop.title
          : config.seo.mainKeyword + " Shop",
      meta: [
        {
          hid: "description",
          name: "description",
          content:
              config.seo.shop && config.seo.shop.metaDescription
                  ? config.seo.shop.metaDescription
                  : config.seo.mainKeyword + " Shop",
        },
        {
          hid: "robots",
          name: "robots",
          content:
              config.seo.shop && config.seo.shop.robots
                  ? config.seo.shop.robots
                  : "noindex, follow",
        },
      ],
    };
  },
  components: {
    Pagination
  },
  data() {
    return {
      products: products,
      minAngle: 0,
      maxAngle: Math.max(...products.map(product => product.price)) + 1,
      max: Math.max(...products.map(product => product.price)) + 1,
      page: 1,
      items: [
        { id: 1, name: 'Sort by price: low to high', isSelected: true, value: 'ascending'},
        { id: 2, name: 'Sort by price: high to low', isSelected: false, value: 'descending' },
      ]
    };
  },
  created() {
    this.products = this.products.sort((a, b) => a.price - b.price);
  },
  methods: {
    sortProducts(direction) {
      if (direction === 'ascending') {
        this.products = this.products.sort((a, b) => a.price - b.price);
      } else {
        this.products = this.products.sort((a, b) => b.price - a.price);
      }
    },
    handleClick(item) {
      this.items.forEach(i => {
        i.isSelected = (i.id === item.id)
      });
      this.sortProducts(item.value);
    },
  },
  computed: {
    sliderMin: {
      get: function () {
        let val = parseInt(this.minAngle);
        return val;
      },
      set: function (val) {
        val = parseInt(val);
        if (val > this.maxAngle) {
          this.maxAngle = val;
        }
        this.minAngle = val;
        this.products = products.filter(product => {
          return Number(product.price) >= this.minAngle && Number(product.price) <= this.maxAngle;
        });
      }
    },
    sliderMax: {
      get: function () {
        let val = parseInt(this.maxAngle);
        return val;
      },
      set: function (val) {
        val = parseInt(val);
        if (val < this.minAngle) {
          this.minAngle = val;
        }
        this.maxAngle = val;

        this.products = products.filter(product => {
          return Number(product.price) >= this.minAngle && Number(product.price) <= this.maxAngle;
        });
      }
    }
  }
};
</script>
