<template>
  <div>
    <div v-if="loading" class="noData">Loading...</div>
    <div v-else-if="error" class="noData">Something went wrong :(</div>
    <div
      v-else-if="shortenLink"
      class="result flex flex-row gap-2 items-center"
    >
      <p
        class="text-white text-xl bg-[#071952] py-2 px-4 md:py-4 md:px-8 shadow-md rounded-md overflow-x-auto"
      >
        {{ shortenLink }}
      </p>
      <button
        @click="copyToClipboard"
        :class="{ copied }"
        class="py-2 px-4 md:p-4 font-semibold bg-[#35A29F] hover:bg-[#35A29F]/80 rounded-md shadow-md"
      >
        {{ copyButton }}
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from "vue";
import { useToast } from "vue-toastification";
import axios from "axios";

const toast = useToast();
const props = defineProps({
  inputValue: {
    type: String,
    required: true,
  },
});

const shortenLink = ref<string>("");
const copied = ref<boolean>(false);
const loading = ref<boolean>(false);
const error = ref<boolean>(false);
const copyButton = ref<string>("Copy");

const generateRandomString = (length: number) => {
  let result = "";
  const characters =
    "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
  const charactersLength = characters.length;

  for (let i = 0; i < length; i++) {
    result += characters.charAt(Math.floor(Math.random() * charactersLength));
  }

  return result;
};

const fetchData = async () => {
  const options = {
    method: "POST",
    url: "https://url-shortener23.p.rapidapi.com/shorten",
    headers: {
      "content-type": "application/json",
      "X-RapidAPI-Key": "1968e2c0e0msh4264307c6bb8ecbp150f50jsn4ea6f907ed95",
      "X-RapidAPI-Host": "url-shortener23.p.rapidapi.com",
    },
    data: {
      url: props.inputValue,
      alias: generateRandomString(8),
    },
  };

  try {
    loading.value = true;
    const res = await axios.request(options);
    console.log(res);
    shortenLink.value = res.data.short_url;
  } catch (err) {
    error.value = true;
  } finally {
    loading.value = false;
  }
};

const copyToClipboard = () => {
  navigator.clipboard.writeText(shortenLink.value);
  copied.value = true;
  setTimeout(() => {
    copied.value = false;
  }, 1000);
  copyButton.value = "Success";
  toast.success("Copied to clipboard!");
};

watch(
  () => props.inputValue,
  () => {
    //@ts-ignore
    if (props.inputValue.length) {
      fetchData();
    }
  }
);
</script>
