<template>
  <button type="button" @click="moveOnTop">
    <font-awesome-icon
      :icon="['fas', 'angle-up']"
      style="color: #f6f9fd"
      class="h-8"
    />
  </button>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  setup(props, context) {
    const moveOnTop = () => {
      context.emit("goTop");
    };

    return {
      moveOnTop,
    };
  },
});
</script>

<style scoped>
button {
  position: fixed;
  bottom: 10px;
  right: 5px;
  border: none;
  outline: none;
  cursor: pointer;
  height: 50px;
  width: 50px;
  opacity: 0.7;
  background-color: #6b9abe;
  border-radius: 4px;
  z-index: 100;
  transition: 0.3s linear;
}

button:hover {
  opacity: 1;
}

@media (min-width: 1024px) {
  button {
    bottom: 70px;
    right: 15px;
  }
}
</style>
