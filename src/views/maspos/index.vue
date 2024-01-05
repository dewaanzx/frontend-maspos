<template>
  <!-- ini menu bar -->
  <div
    class="w-screen h-[80px] mt-[-20px] ml-[-20px] flex items-center bg-[#fafafa]"
  >
    <div class="ml-[1000px]">
      <button
        class="w-36 h-10 rounded-[3px] font-medium bg-[#B3D4FF] text-sm text-[#0052CC] hover:bg-blue-300"
        @click="$router.push('/maspos/addcategory')"
      >
        + Add Category
      </button>
      <button
        class="w-36 h-10 rounded-[3px] ml-2 font-medium bg-[#B3D4FF] text-sm text-[#0052CC] hover:bg-blue-300"
        @click="$router.push('/maspos/addproduct')"
      >
        + Add Product
      </button>
      <button
        class="w-[3.475rem] h-10 rounded-[3px] ml-2 font-medium bg-[#0052CC] text-sm text-white hover:bg-blue-800"
        @click="$router.push('/maspos/cart')"
      >
        Cart
      </button>
    </div>
  </div>

  <!-- ini tab bar -->
  <div
    class="border-b-2 border-[##EBECF0] mb-2 ml-[130px] w-[1210px] bg-[#fafafa]"
  >
    <div class="ml-[2px]">
      <button
        class="ml-3 text-sm text-[#7A869A] rown"
        v-for="(category, index) in categoryStore.categorys"
        :key="category.categorys"
        @click="fetchByCategory(category.id)"
      >
        <!-- {{ index }}: {{ category.categorys }} -->
        <RouterLink
          :to="'/maspos/' + category.id"
          :class="$route.params.id == category.id ? 'text-[#0052CC]' : ''"
          >{{ category.name }}</RouterLink
        >

        <!-- <a :href="'/maspos/' + category.id">{{ category.name }} </a> -->
      </button>
    </div>
  </div>

  <!-- ini card product -->
  <div class="bg-[#fafafa] h-screen w-screen ml-[-20px] mt-[-8px]">
    <div class="px-[150px] pt-[15px] flex flex-wrap">
      <!-- ini bagian card -->
      <div
        v-for="product in productStore.products"
        class="w-[200px] mh-[300px] bg-white rounded-[10px] drop-shadow-lg mr-11 mt-10"
        style="z-index: 34"
      >
        <div class="w-[200px] h-[165px] bg-red-200 rounded-t-[10px]">
          <img
            :src="'http://localhost:8080/uploads/' + product.image"
            class="w-[200px] h-[165px] rounded-t-[10px]"
            style="object-fit: cover"
            alt=""
          />
        </div>
        <!-- ini nama product -->
        <div class="flex px-[15px] pt-2">
          <p class="text-[14px] w-[134px]" style="min-height: 45px">
            {{ product.name }}
          </p>
          <button
            class="bg-red-600 rounded-sm w-[36px] h-[20px] text-[8px] text-white"
            @click.prevent="deleteData(product.id)"
          >
            Delete
          </button>
        </div>
        <!-- ini harga -->
        <H1 class="px-[15px] font-bold">
          {{ formatCurrency(product.price) }}</H1
        >
        <!-- ini button -->
        <div class="py-[10px] mt-5 flex justify-center">
          <button
            class="bg-[#0052CC] w-[100px] h-[32px] text-white rounded text-[12px] justify-center"
            @click="create(product.id)"
          >
            + Add to Cart
          </button>
        </div>
      </div>
    </div>
    <div class="fixed ml-[1160px]">
      <button
        class="h-[59px] w-[205px] bg-[#0052CC] rounded-md text-white text-[16px] font-semibold mt-16"
        @click="$router.push('/maspos/cart')"
      >
        Total Bill : {{ formatCurrency(cartStore.carts.total) }}
      </button>
    </div>
  </div>
</template>

<script>
import { useCategoryStore } from "@/stores/category.store.js";
import { useProductStore } from "@/stores/product.store";
import { useCartStore } from "@/stores/cart.store";

export default {
  data() {
    return {
      categoryStore: useCategoryStore(),
      productStore: useProductStore(),
      cartStore: useCartStore(),
      formData: {
        product_id: null,
      },
    };
  },
  methods: {
    async create(id) {
      this.formData.product_id = id;
      let cart = await this.cartStore.add(this.formData);

      if (cart) {
        this.$router.push("/maspos/cart/");
      }
    },
    async deleteData(id) {
      this.$swal({
        title: "Are you sure?",
        text: "Delete Product",
        icon: "warning",
        showCancelButton: true,
        confirmButtonText: "Yes, delete it!",
      }).then(async (result) => {
        if (result.isConfirmed) {
          await this.productStore.delete(id);

          const category_id = this.$route.params.id;
          this.productStore.fetch(category_id);
        }
      });
    },
    formatCurrency(price) {
      if (price === null || price === undefined) {
        return "0";
      }

      return price.toLocaleString("id-ID", {
        style: "currency",
        currency: "IDR",
        minimumFractionDigits: 0,
      });
    },
    fetchByCategory(id) {
      this.productStore.fetch(id);
    },
  },
  mounted() {
    this.categoryStore.fetch();
    this.cartStore.fetch();
    const id = this.$route.params.id;
    this.productStore.fetch(id);
  },
};
</script>

<style>
.rown {
  @apply px-2 cursor-pointer hover:text-[#0052CC];
}
</style>
