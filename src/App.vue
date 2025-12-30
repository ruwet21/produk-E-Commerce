<script setup>
import { ref, computed } from "vue";

// Data Produk
const product = {
  name: "Nike Air Zoom Pegasus 39",
  price: "Rp 2.499.000",
  description:
    "Sepatu lari yang responsif dan tahan lama, cocok untuk latihan sehari-hari.",
  variants: [
    {
      id: 1,
      colorName: "Midnight Navy",
      hex: "bg-blue-900",
      image: "https://placehold.co/600x400/1e3a8a/FFFFFF?text=Navy+Shoe",
      stock: 5,
    },
    {
      id: 2,
      colorName: "Crimson Red",
      hex: "bg-red-600",
      image: "https://placehold.co/600x400/dc2626/FFFFFF?text=Red+Shoe",
      stock: 0,
    },
    {
      id: 3,
      colorName: "Raven Black",
      hex: "bg-gray-900",
      image: "https://placehold.co/600x400/111827/FFFFFF?text=Black+Shoe",
      stock: 12,
    },
  ],
};

// State Reactive
const selectedVariant = ref(product.variants[0]);
const quantity = ref(1);

// Computed untuk status stok
const isOutOfStock = computed(() => {
  return selectedVariant.value.stock === 0;
});

// Fungsi Event Handler
const selectColor = (variant) => {
  selectedVariant.value = variant;
  quantity.value = 1; // Reset quantity saat ganti warna
};

const addToCart = () => {
  alert(
    `Berhasil menambahkan ${quantity.value} pasang ${product.name} (${selectedVariant.value.colorName}) ke keranjang!`
  );
};
</script>

<template>
  <div class="min-h-screen bg-gray-100 flex items-center justify-center p-6">
    <div
      class="bg-white rounded-2xl shadow-xl overflow-hidden max-w-4xl w-full flex flex-col md:flex-row"
    >
      <div class="w-full md:w-1/2 h-64 md:h-auto bg-gray-200 relative">
        <img
          :src="selectedVariant.image"
          :alt="product.name"
          class="w-full h-full object-cover transition-opacity duration-300"
        />
        <span
          v-if="isOutOfStock"
          class="absolute top-4 left-4 bg-red-500 text-white px-3 py-1 rounded-full text-xs font-bold tracking-wide uppercase"
        >
          Stok Habis
        </span>
      </div>

      <div class="w-full md:w-1/2 p-8 flex flex-col justify-between">
        <div>
          <h2 class="text-3xl font-bold text-gray-800 mb-2">
            {{ product.name }}
          </h2>
          <p class="text-xl text-emerald-600 font-semibold mb-4">
            {{ product.price }}
          </p>
          <p class="text-gray-500 leading-relaxed mb-6">
            {{ product.description }}
          </p>

          <div class="mb-6">
            <h3 class="text-sm font-medium text-gray-900 mb-3">
              Pilih Warna:
              <span class="text-gray-500 font-normal">{{
                selectedVariant.colorName
              }}</span>
            </h3>
            <div class="flex space-x-3">
              <button
                v-for="variant in product.variants"
                :key="variant.id"
                @click="selectColor(variant)"
                :class="[
                  variant.hex,
                  'w-10 h-10 rounded-full border-2 focus:outline-none transition-transform active:scale-95',
                  selectedVariant.id === variant.id
                    ? 'border-gray-800 ring-2 ring-gray-300 ring-offset-2'
                    : 'border-transparent',
                ]"
                :aria-label="variant.colorName"
              ></button>
            </div>
          </div>
        </div>

        <div class="flex items-center gap-4 pt-6 border-t border-gray-100">
          <div class="flex items-center border border-gray-300 rounded-lg">
            <button
              @click="quantity > 1 ? quantity-- : null"
              class="px-4 py-2 hover:bg-gray-50 text-gray-600 disabled:opacity-50"
              :disabled="isOutOfStock"
            >
              -
            </button>
            <span class="px-4 font-medium text-gray-900">{{ quantity }}</span>
            <button
              @click="quantity < selectedVariant.stock ? quantity++ : null"
              class="px-4 py-2 hover:bg-gray-50 text-gray-600 disabled:opacity-50"
              :disabled="isOutOfStock"
            >
              +
            </button>
          </div>

          <button
            @click="addToCart"
            :disabled="isOutOfStock"
            :class="[
              'flex-1 py-3 px-6 rounded-lg font-semibold shadow-md transition-all',
              isOutOfStock
                ? 'bg-gray-300 text-gray-500 cursor-not-allowed'
                : 'bg-gray-900 text-white hover:bg-gray-800 hover:shadow-lg active:scale-98',
            ]"
          >
            {{ isOutOfStock ? "Stok Habis" : "Beli Sekarang" }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
