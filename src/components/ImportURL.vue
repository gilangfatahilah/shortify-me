<template>
  <div>
    <h1
      class="text-center text-4xl md:text-6xl pb-4 font-bold bg-clip-text text-transparent bg-gradient-to-r from-white to-teal-500"
    >
      SHORTIFY.ME
    </h1>
    <div class="flex flex-row justify-evenly items-center gap-2 md:gap-0 my-4">
      <input
        type="text"
        placeholder="Enter URL here..."
        class="py-2 px-4 md:py-4 md:px-8 rounded-md text-white bg-[#35A29F] placeholder:text-white placeholder:font-semibold shadow-md"
        v-model="link"
      />
      <button
        @click="handleClick"
        class="py-2 px-4 md:p-4 font-semibold bg-[#071952] hover:bg-[#071952]/80 rounded-md shadow-md"
      >
        Shorten
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();
const link = ref<string>("");
const emit = defineEmits(["linkSubmitted"]);

const handleClick = (): void => {
  if (!link.value) {
    toast.error("Please enter a link to shorten!");
  }

  emit("linkSubmitted", link.value);
  link.value = "";
};
</script>
