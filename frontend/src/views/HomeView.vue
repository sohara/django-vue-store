<template>
  <div class="bg-white">
    <div class="mx-auto max-w-2xl px-4 py-16 sm:px-6 sm:py-24 lg:max-w-7xl lg:px-8">
      <h2 class="text-2xl font-bold tracking-tight text-gray-900">Recent Products</h2>

      <div class="mt-6 grid grid-cols-1 gap-x-6 gap-y-10 sm:grid-cols-2 lg:grid-cols-4 xl:gap-x-8">
        <div v-for="product in latestProducts" :key="product.id" class="group relative">
          <div
            class="aspect-h-1 aspect-w-1 w-full overflow-hidden rounded-md bg-gray-200 lg:aspect-none group-hover:opacity-75 lg:h-80"
          >
            <img
              :src="product.get_image"
              :alt="product.name"
              class="h-full w-full object-cover object-center lg:h-full lg:w-full"
            />
          </div>
          <div class="mt-4 flex justify-between">
            <div>
              <h3 class="text-sm text-gray-700">
                <a :href="product.get_absolute_url">
                  <span aria-hidden="true" class="absolute inset-0" />
                  {{ product.name }}
                </a>
              </h3>
              <p class="mt-1 text-sm text-gray-500">{{ product.description }}</p>
            </div>
            <p class="text-sm font-medium text-gray-900">{{ product.price }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
type Product = {
  id: number;
  name: string;
  get_absolute_url: string;
  description: string;
  price: string;
  get_image: string;
  get_thumbnail: string;
};

export default {
  name: 'HomeView',
  data(): { latestProducts: Product[] } {
    return {
      latestProducts: []
    };
  },
  components: {},
  mounted() {
    this.getLatestProducts();
  },
  methods: {
    async getLatestProducts() {
      try {
        const response = await fetch('http://localhost:8000/api/v1/latest-products/');
        const json = await response.json();
        this.latestProducts = json as Product[];
      } catch (e) {
        console.log('ERROR', e);
      }
    }
  }
};
</script>
