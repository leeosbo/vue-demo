<script setup>
defineProps({
  msg: {
    type: String,
    required: true,
  },
});
</script>

<template>
  <div class="header">
    <h1 class="green">{{ msg }}</h1>
  </div>
</template>

<style scoped>
.header {
  text-align: center;
}
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}
</style>
