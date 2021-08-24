<template>
  <Form />
</template>

<script>
import Form from "@/components/Form.vue";

export default {
  name: "App",
  components: {
    Form,
  },
};
</script>

<style>
body {
  display: flex;
  align-items: center;
  padding-left: 25rem !important;
  padding-top: 40px;
  padding-bottom: 40px;
  background-color: #f5f5f5;
}

@media (max-width: 500px) {
  body{
    padding-left: 25px !important;
  }
}
</style>