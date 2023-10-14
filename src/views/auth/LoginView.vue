<script setup>
import { inject } from "vue";
import { useRouter } from "vue-router";
import AuthAPI from "../../api/AuthAPI";
import { FormKit } from "@formkit/vue";

const toast = inject("toast");
const router = useRouter();

const handleSubmit = async (formData) => {
  try {
    const {
      data: { token },
    } = await AuthAPI.login(formData);
    localStorage.setItem("AUTH_TOKEN", token);
    router.push({ name: "my-appointments" });
  } catch (error) {
    toast.open({
      message: error.response.data.msg,
      type: "error",
    });
  }
};
</script>
<template>
  <h1 class="text-6xl font-extrabold text-white text-center mt-10">
    Iniciar Sesión
  </h1>
  <p class="text-2xl text-white text-center my-5">
    Si tienes una cuenta, inicia sesión
  </p>

  <FormKit
    id="loginForm"
    type="form"
    :actions="false"
    incomplete-message="No se pudo enviar, revisa las notificaciones"
    @submit="handleSubmit"
  >
    <FormKit
      type="email"
      label="Email"
      name="email"
      placeholder="Email de Usuario"
      validation="required|email"
      :validation-messages="{
        required: 'El Email es obligatorio',
        email: 'Email no válido',
      }"
    ></FormKit>
    <FormKit
      type="password"
      label="Password"
      name="password"
      placeholder="Password de Usuario"
      validation="required"
      :validation-messages="{
        required: 'El Password es obligatorio',
      }"
    ></FormKit>

    <FormKit type="submit">Iniciar Sesión</FormKit>
  </FormKit>
</template>
