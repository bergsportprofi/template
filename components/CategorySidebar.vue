<template>
  <div>
    <div class="row">
      <div class="d-flex flex-column flex-shrink-0 p-3 text-white bg-dark">

          <!-- <a
        href="/"
        class="
          d-flex
          align-items-center
          mb-3 mb-md-0
          me-md-auto
          text-white text-decoration-none
        "
      >
        <span class="fs-4">Kategorien</span>
      </a> -->
          <!-- <hr />
          <span class="fs-4">Kategorien</span>
          <ul class="nav nav-pills flex-column mb-auto">
            <li
              v-for="(category, index) in categories"
              :key="index"
              class="nav-item"
            >
              <nuxt-link
                :title="category.seo ? category.seo.title : category.name"
                :to="
                  category.type === 'manufacturer'
                    ? `/marken/${category.key}/`
                    : `/kategorie/${category.key}/`
                "
                class="nav-link text-white"
              >
                {{ category.name }}
              </nuxt-link>
            </li>
          </ul>
          <hr />

          <span class="fs-4">Hersteller</span>

          <ul class="nav nav-pills flex-column mb-auto">
            <li
              v-for="(brand, index) in config.brands"
              :key="index"
              class="nav-item"
            >
              <nuxt-link
                :title="brand.seo ? brand.seo.title : brand.name"
                :to="
                  brand.type === 'manufacturer'
                    ? `/marken/${brand.key}/`
                    : `/marken/${brand.key}/`
                "
                class="nav-link text-white"
              >
                {{ brand.name }}
              </nuxt-link>
            </li>
          </ul>
          <hr /> -->
        <div
            class="dropdown"
            v-for="(category, index) in categories"
            :key="index"
        >
          <nuxt-link
              v-if="!categories.some(c => c.subCategories.includes(category.slug))"
              :to="`/kategorie/${category.slug}/`"
              :title="`${category.name}`"
              class="d-flex align-items-center text-white text-decoration-none"
              aria-expanded="false"
          >
            <strong>{{ category.name }}</strong>
          </nuxt-link>
          <ul v-if="category.subCategories.length > 0">
            <li v-for="subCategory in category.subCategories" :key="subCategory">
              <nuxt-link
                  :to="`/kategorie/${subCategory}/`"
                  :title="`${subCategory}`"
                  class="d-flex align-items-center text-white text-decoration-none"
                  aria-expanded="false"
              >
                {{ subCategory }}
              </nuxt-link>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import config from "~/assets/data/config.json";
import categories from "~/assets/data/categories.json";
import products from "~/assets/data/products.json";

// we want unique values in the brands array
function onlyUnique(value, index, self) {
  return self.indexOf(value) === index;
}

let brands = [];
for (let i = 0; i <= products.length; i++) {
  try {
    brands.push(products[i].brand);
  } catch {
    // console.log("no brands key");
  }
}

brands = brands.filter(onlyUnique);

export default {
  name: "categorySidebar",
  data() {
    return {
      config,
      categories,
    };
  },
};
</script>
