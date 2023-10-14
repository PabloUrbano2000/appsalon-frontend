<script setup>
import { onMounted, inject } from "vue";
import { useRoute, useRouter } from "vue-router";
import AuthAPI from "../../api/AuthAPI";

const toast = inject("toast");
const route = useRoute();
const router = useRouter();
const { token } = route.params;

onMounted(async () => {
  try {
    const { data } = await AuthAPI.verifyAccount(token);
    toast.open({
      message: data.msg,
      type: "success",
    });
    setTimeout(() => {
      router.push({ name: "login" });
    }, 4000);
  } catch (error) {
    console.log(error);
    toast.open({
      message: error.response.data.msg,
      type: "error",
    });
  }
});
</script>
<template>
  <h1 class="text-6xl font-extrabold text-white text-center mt-10">
    Verificar Cuenta
  </h1>
</template>
