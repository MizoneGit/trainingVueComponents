<template>
  <button class="btn" >
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: "my-button",
}
</script>

<style scoped>
.btn {
  padding: 15px 30px;
  color: #FFFFFF;
  background-color: #3A68B8;
  border: none;
  border-radius: 5px;
  transition: linear 0.3s;
  cursor: pointer;
}
.btn:hover {
  background-color: #305597;
}
</style>