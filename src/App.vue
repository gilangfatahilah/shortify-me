<template>
  <div class="w-1/4 h-screen mx-auto flex flex-col justify-center items-center">
    <ImportURL @linkSubmitted="handleLinkSubmitted" />
    <LinkResult :inputValue="link" />
  </div>
  <Background />
</template>

<script lang="ts" setup>
import Background from "./components/Background.vue";
import ImportURL from "./components/ImportURL.vue";
import LinkResult from "./components/LinkResult.vue";

import { ref } from "vue";

const link = ref<string>("");

const handleLinkSubmitted = (value: any) => {
  link.value = value;
  console.log(link.value);
};
</script>
