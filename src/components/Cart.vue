<script setup>
import { computed, inject } from "vue";

import Popper from "vue3-popper";
import CartItem from "./CartItem.vue";

const props = defineProps({
  cart: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(["decrease-quantity", "increase-quantity"]);

const emptyCart = inject("emptyCart");

const totalAmount = computed(() => {
  const total = props.cart.reduce(
    (total, item) => total + item.quantity * item.price,
    0
  );

  return total;
});
</script>

<template>
  <Popper>
    <button
      class="cart-button"
      :data-items="props.cart.length"
      :class="{ 'has-items': props.cart.length }"
      type="button"
    >
      <i class="bi bi-cart4"></i>
    </button>
    <template #content>
      <div class="cart-resume">
        <p class="cart-empty" v-if="!cart.length">El carrito esta vácio</p>
        <table v-else class="cart-table">
          <thead>
            <tr>
              <th>
                <div>
                  <div>
                    <span> Nombre </span>
                  </div>
                </div>
              </th>
              <th>
                <div>
                  <div>
                    <span> Precio </span>
                  </div>
                </div>
              </th>
              <th>
                <div>
                  <div>
                    <span> Cantidad </span>
                  </div>
                </div>
              </th>
            </tr>
          </thead>
          <tbody>
            <CartItem
              v-for="item of cart"
              :key="item.id"
              :item="item"
              @decrease-quantity="(item) => emit('decrease-quantity', item)"
              @increase-quantity="(item) => emit('increase-quantity', item)"
            />
          </tbody>
          <tfoot>
            <tr>
              <td colspan="3">
                <div>
                  <div>
                    <p class="cart-total">
                      Total a pagar: <span>${{ totalAmount }}</span>
                    </p>
                    <button class="cart-reset" @click="emptyCart">
                      Vaciar carrito
                    </button>
                  </div>
                </div>
              </td>
            </tr>
          </tfoot>
        </table>
      </div>
    </template>
  </Popper>
</template>

<style scoped>
.cart-button {
  all: unset;
  display: flex;
  align-items: center;
  cursor: pointer;
  justify-content: center;
  font-size: 2rem;
  padding: 0.5rem;
  color: #fff;
}

.cart-button.has-items {
  position: relative;
}
.cart-button.has-items::after {
  content: attr(data-items);
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: tomato;
  border-radius: 9999px;
  color: #fff;
  font-size: 0.75rem;
  height: 20px;
  position: absolute;
  right: 0;
  top: 8px;
  width: 20px;
}

.cart-resume {
  background-color: #fff;
  border-radius: 0.25rem;
  padding: 1rem;
  position: relative;
  width: 420px;
}
.cart-resume::after {
  background-color: #fff;
  content: "";
  height: 12px;
  left: 0;
  margin: auto;
  position: absolute;
  right: 0;
  top: -6px;
  transform: rotate(45deg);
  width: 12px;
}

.cart-empty {
  margin: 0 0 0;
  text-align: center;
}
.cart-reset {
  display: block;
  background-color: var(--color-gray-900);
  color: var(--color-white);
  font-weight: 900;
  padding: 0.5rem 1rem;
  text-transform: uppercase;
  width: 100%;
}

.cart-table {
  width: 100%;
}
.cart-table thead span {
  display: block;
  text-align: left;
}
</style>
