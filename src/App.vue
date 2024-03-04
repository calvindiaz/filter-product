<script setup>
import { computed, ref } from "vue";
import products from "./data";

const cartCount = ref(0);
const cartContent = ref([]);
const searchContent = ref("");
const checkedCategories = ref([]);

// 匹配搜尋的商品
// const matchedProducts = computed(() => {
//   if (!searchValue.value.toLowerCase()) return products;
//   return products.filter((product) =>
//     product.name.toLowerCase().includes(searchValue.value.toLowerCase())
//   );
// });

// 匹配分類的商品
// const matchedCategories = computed(() => {
//   if (!checkedCategories.value.length) {
//     return products;
//   }
//   return products.filter((product) =>
//     checkedCategories.value.includes(product.category)
//   );
// });

// 把上面兩種filter合併成一個
const matched = computed(() => {
  let result = products;
  if (searchContent.value) {
    result = result.filter((product) =>
      product.name.toLowerCase().includes(searchContent.value.toLowerCase())
    );
  }
  if (checkedCategories.value.length) {
    result = result.filter((product) =>
      checkedCategories.value.includes(product.category)
    );
    console.log(checkedCategories.value);
  }
  return result;
});

// 計算購物車商品數量，單純能動
const add = (product) => {
  cartContent.value.push(product);
  cartCount.value = cartContent.value.length;
};
</script>

<template>
  <main class="bg-gray-800 text-white">
    <nav class="bg-gray-900 p-4 mb-6">
      <div
        class="container max-w-6xl mx-auto flex flex-col sm:flex-row items-center"
      >
        <!-- Search Area -->
        <div class="relative w-full">
          <input
            type="text"
            id="search"
            class="bg-gray-700 rounded-full p-2 pl-10 focus:w-3/5"
            placeholder="Search Product..."
            v-model.trim="searchValue"
          />
          <svg
            class="absolute left-2 top-1/2 -translate-y-1/2"
            xmlns="http://www.w3.org/2000/svg"
            width="20"
            height="20"
            viewBox="0 0 24 24"
            stroke-width="2"
            stroke="currentColor"
            fill="none"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path stroke="none" d="M0 0h24v24H0z" fill="none" />
            <path d="M10 10m-7 0a7 7 0 1 0 14 0a7 7 0 1 0 -14 0" />
            <path d="M21 21l-6 -6" />
          </svg>
        </div>
        <!-- Cart Icon -->
        <div class="relative text-center">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            viewBox="0 0 24 24"
            stroke-width="2"
            stroke="currentColor"
            fill="none"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path stroke="none" d="M0 0h24v24H0z" fill="none" />
            <path
              d="M6.331 8h11.339a2 2 0 0 1 1.977 2.304l-1.255 8.152a3 3 0 0 1 -2.966 2.544h-6.852a3 3 0 0 1 -2.965 -2.544l-1.255 -8.152a2 2 0 0 1 1.977 -2.304z"
            />
            <path d="M9 11v-5a3 3 0 0 1 6 0v5" />
          </svg>
          <small
            id="cart-count"
            class="bg-red-500 px-1 rounded-full text-xs text-white absolute -top-2 -right-2"
            >{{ cartCount }}</small
          >
        </div>
      </div>
    </nav>

    <section class="container max-w-6xl flex flex-col md:flex-row mx-auto">
      <div class="space-y-4 p-2 w-full max-w-[10rem]">
        <h2 class="text-2xl">Filters</h2>
        <h3 class="text-xl">Category</h3>
        <div id="filters-container" class="text-xl space-y-2">
          <div>
            <input
              type="checkbox"
              id="cameras"
              class="check"
              value="cameras"
              v-model="checkedCategories"
            />
            <label for="cameras" class="ml-1">Cameras</label>
          </div>
          <div>
            <input
              type="checkbox"
              id="games"
              class="check"
              value="games"
              v-model="checkedCategories"
            />
            <label for="games" class="ml-1">Games</label>
          </div>
          <div>
            <input
              type="checkbox"
              id="smartphones"
              class="check"
              value="smartphones"
              v-model="checkedCategories"
            />
            <label for="smartphones" class="ml-1">Smartphones</label>
          </div>
          <div>
            <input
              type="checkbox"
              id="televisions"
              class="check"
              value="televisions"
              v-model="checkedCategories"
            />
            <label for="televisions" class="ml-1">Televisions</label>
          </div>
        </div>
      </div>
      <!-- Product Wrapper -->
      <div
        id="products-wrapper"
        class="w-full max-w-4xl mx-auto grid grid-cols-2 sm:grid-cols-3 xl:grid-cols-4 gap-6 place-content-center p-4"
      >
        <div
          v-for="product in matched"
          :key="product.name"
          class="item space-y-2"
        >
          <div
            class="bg-gray-100 flex justify-center relative overflow-hidden group cursor-pointer border rounded-xl"
          >
            <img :src="product.url" class="w-full h-full object-cover" />
            <button
              @click="add(product)"
              class="status bg-black text-white absolute bottom-0 left-0 right-0 text-center py-2 translate-y-full transition group-hover:translate-y-0"
            >
              Add To Cart
            </button>
          </div>
          <p class="text-xl">{{ product.name }}</p>
          <strong>${{ product.price }}</strong>
        </div>
      </div>
    </section>
  </main>
</template>

<style scoped></style>
