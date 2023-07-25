<script setup>
import Cart from "./Cart.vue";

const props = defineProps({
  cart: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(["decrease-quantity", "increase-quantity"]);
</script>

<template>
  <header>
    <div>
      <a class="logo">
        <img src="/img/logo.svg" alt="GuitarLA Logo" />
      </a>
      <nav>
        <Cart
          :cart="cart"
          @decrease-quantity="(item) => emit('decrease-quantity', item)"
          @increase-quantity="(item) => emit('increase-quantity', item)"
        />
      </nav>
    </div>
  </header>
</template>

<style scoped>
header {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 999;
}
header > div {
  display: flex;
  align-items: center;
  justify-content: start;
  gap: 4rem;
  padding: 1.75rem 3rem;
}
.logo {
  display: block;
  min-width: 180px;
}
.logo > img {
  display: block;
  height: auto;
  max-width: 100%;
}
</style>
