<template>
    <button @click="goBack" class="" style="border: none;"><i class="fa-sharp fa-solid fa-arrow-left"></i></button>
  </template>
  
  <script>
  export default {
    methods: {
      goBack() {
        this.$router.go(-1);
      },
    },
  };
  </script>