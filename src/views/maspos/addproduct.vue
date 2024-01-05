<template>
  <div
    class="bg-[#fafafa] w-screen h-screen ml-[-20px] mt-[-20px] flex justify-center items-center"
  >
    <div action="" class="bg-white w-[790px] h-[600px] rounded-md shadow">
      <h1
        class="h-[50px] text-[20px] font-semibold text-center my-[30px] border-b-2"
      >
        Add Product
      </h1>
      <div class="justify-end items-center">
        <form
          action=""
          @submit.prevent="create"
          enctype="multipart/form-data"
          class="flex justify-center"
        >
          <div>
            <label
              for="imageinput"
              class="w-[180px] h-[180px] flex items-center justify-center bg-[#B3D4FF] rounded-md cursor-pointer hover:bg-blue-300 opacity-60"
            >
              <div class="grid justify-items-center space-y-2">
                <img src="../../assets/upload.png" alt="" />
                <p class="text-[12px] text-[#1963D2]">Upload Image</p>
              </div>
            </label>
            <input
              type="file"
              class="file:hidden"
              id="imageinput"
              @change="handleFileChange"
              accept=".jpg, .jpeg, .png"
            />
          </div>
          <div class="ml-[10px]">
            <label for="#" class="block pl-[80px]">
              <span class="block text-[12px] text-slate-700">Product Name</span>
              <input
                v-model="formData.name"
                type="text"
                class="border w-[340px] h-[44px] pl-2 rounded-sm"
                required
              />
            </label>
            <label for="#" class="block pl-[80px] pt-[20px]">
              <span class="block text-[12px] text-slate-700">Price</span>
              <input
                v-model="formData.price"
                type="text"
                class="border w-[340px] h-[44px] pl-2 rounded-sm"
                required
              />
            </label>
            <label for="#" class="block pl-[80px] pt-[20px]">
              <span class="block text-[12px] text-slate-700"
                >Select Category</span
              >
              <select
                v-model="formData.category_id"
                class="border w-[340px] h-[44px] pl-2 rounded-sm"
              >
                <option
                  v-for="category in categoryStore.categorys"
                  :value="category.id"
                  :key="category.id"
                >
                  {{ category.name }}
                </option>
              </select>
            </label>
            <!-- bagian button -->
            <div class="flex justify-end items-center pt-[160px] space-x-4">
              <button
                class="w-[107px] h-[40px] text-[14px] text-[#1963D2] border-2 border-[#1963D2] rounded-md hover:bg-blue-100"
                @click="$router.push('/maspos/1')"
              >
                Cancel
              </button>
              <button
                class="w-[107px] h-[40px] text-[14px] text-white bg-[#1963D2] rounded-md hover:bg-blue-600"
              >
                Confirm
              </button>
            </div>
          </div>
        </form>
      </div>
    </div>
    <!-- <form @submit.prevent="create" enctype="multipart/form-data">
      <label for="">Product</label>
      <div>
        kategori
        <select v-model="formData.category_id">
          <option
            v-for="category in categoryStore.categorys"
            :value="category.id"
            :key="category.id"
          >
            {{ category.name }}
          </option>
        </select>
      </div>
      <div>
        <input
          v-model="formData.name"
          type="text"
          placeholder="tulis product"
        />
      </div>
      <div>
        <input v-model="formData.price" type="text" placeholder="tulis harga" />
      </div>
      <div>
        <input
          type="file"
          @change="handleFileChange"
          accept=".jpg, .jpeg, .png"
        />
      </div>
      <button class="w-[100px] h-100px bg-blue-400">Tambah</button>
    </form> -->
  </div>
</template>

<script>
import { useProductStore } from "@/stores/product.store.js";
import { useCategoryStore } from "@/stores/category.store.js";

export default {
  data() {
    return {
      categoryStore: useCategoryStore(),
      productStore: useProductStore(),
      formData: {
        name: null,
        category_id: null,
        price: null,
        image: null,
      },
    };
  },
  mounted() {
    this.categoryStore.fetch();
  },
  methods: {
    handleFileChange(e) {
      const image = e.target.files[0];
      this.formData.image = image;
    },
    async create() {
      let product = await this.productStore.add(this.formData);

      if (product) {
        this.$router.push("/maspos/" + this.formData.category_id);
      }
    },
  },
};
</script>
