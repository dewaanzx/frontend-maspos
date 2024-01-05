<template>
  <div
    class="bg-[#fafafa] w-screen h-screen ml-[-20px] mt-[-20px] flex justify-center items-center"
  >
    <div
      action=""
      class="bg-white w-[500px] h-[300px] rounded-md shadow mt-[-150px]"
    >
      <h1
        class="h-[50px] text-[20px] font-semibold text-center my-[30px] border-b-2"
      >
        Add Category
      </h1>
      <div class="justify-center items-center">
        <form action="" @submit.prevent="create">
          <label for="#" class="block pl-[80px] pt-[20px]">
            <span class="block text-[12px] text-slate-700">Category Name</span>
            <input
              v-model="formData.name"
              type="text"
              class="border w-[340px] h-[44px] pl-2"
              required
            />
          </label>
          <!-- bagian button -->
          <div class="flex justify-center items-center pt-10 space-x-4">
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
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import { useCategoryStore } from "@/stores/category.store.js";

export default {
  data() {
    return {
      categoryStore: useCategoryStore(),
      formData: {
        name: null,
      },
    };
  },
  methods: {
    async create() {
      let category = await this.categoryStore.add(this.formData);

      if (category) {
        this.$router.push("/maspos/1");
      }
    },
  },
};
</script>
