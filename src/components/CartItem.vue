<script setup>
import { inject } from "vue";

const props = defineProps({
  item: {
    type: Object,
    required: true,
  },
});

const emit = defineEmits(["decrease-quantity", "increase-quantity"]);
const removeGuitar = inject("removeGuitar");
</script>

<template>
  <tr>
    <td>
      <div class="cart-cell-wrapper">
        <div class="cart-cell-content">
          <img
            :alt="item.name"
            :src="`/img/${item.image}.jpg`"
            class="cart-thumbnail"
          />
          <p class="cart-p-name">
            {{ item.name }}
          </p>
        </div>
      </div>
    </td>
    <td>
      <div class="cart-cell-wrapper">
        <div class="cart-cell-content">
          <p class="cart-p-price">${{ item.price }}</p>
        </div>
      </div>
    </td>
    <td>
      <div class="cart-cell-wrapper">
        <div class="cart-cell-content">
          <div class="cart-p-counter">
            <button @click="() => emit('decrease-quantity', item.id)">-</button>
            <input
              type="number"
              :value="item.quantity > 1 ? item.quantity : 1"
              @input="(e) => (item.quantity = e.target.valueAsNumber)"
            />
            <button @click="() => emit('increase-quantity', item.id)">+</button>
          </div>
          <button class="cart-p-remove" @click="() => removeGuitar(item.id)">
            <i class="bi bi-x-circle-fill"></i>
          </button>
        </div>
      </div>
    </td>
  </tr>
</template>

<style scoped>
.cart-cell-content {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.cart-thumbnail {
  display: block;
  width: 100%;
  max-width: 32px;
}
.cart-total {
  margin: 0;
  text-align: right;
}
.cart-p-remove {
  all: unset;
  display: block;
  cursor: pointer;
  color: tomato;
  font-size: 1.5rem;
}
.cart-p-name {
  font-weight: 400;
  margin: 0;
  text-transform: uppercase;
}
.cart-p-price {
  font-weight: 700;
  margin: 0;
  text-transform: uppercase;
}
.cart-p-counter {
  display: flex;
  align-items: center;
  justify-content: start;
}
.cart-p-counter input {
  border: 1px solid var(--color-gray-400);
  height: 28px;
  max-width: 60px;
  width: 100%;
  outline: none;
}
.cart-p-counter button {
  all: unset;
  display: flex;
  align-items: center;
  background-color: var(--color-gray-900);
  color: var(--color-white);
  cursor: pointer;
  font-size: 1.125rem;
  height: 28px;
  justify-content: center;
  width: 28px;
}
</style>
