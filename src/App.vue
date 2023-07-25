<script setup>
import { onMounted, provide, ref, watch } from "vue";
import Header from "./components/Header.vue";
import Guitar from "./components/Guitar.vue";
import { guitarsData } from "./data/guitars";

const guitar = ref({});
const guitars = ref([]);
const shoppingCart = ref([]);

// Init data with guitar
onMounted(() => {
  guitars.value = guitarsData;
  guitar.value = guitarsData[0];

  const cartTrack = window.localStorage.getItem("shoppingCart");
  cartTrack && (shoppingCart.value = JSON.parse(cartTrack));
});

const trackingCart = () => {
  window.localStorage.setItem(
    "shoppingCart",
    JSON.stringify(shoppingCart.value)
  );
};

const guitarOverview = (guitarSelected) => {
  guitar.value = { ...guitarSelected };
};

const addGuitarToCart = (guitar) => {
  const guitarIndex = shoppingCart.value.findIndex(
    ({ id }) => id === guitar.id
  );

  guitarIndex > -1
    ? shoppingCart.value[guitarIndex].quantity++
    : (shoppingCart.value = [
        ...shoppingCart.value,
        { ...guitar, quantity: 1 },
      ]);
};

const removeGuitarToCart = (id) => {
  shoppingCart.value = shoppingCart.value.filter((item) => item.id !== id);
};

const emptyCart = () => {
  shoppingCart.value = [];
};

const decreaseGuitarQuantity = (id) => {
  shoppingCart.value = shoppingCart.value.map((item) => {
    if (item.id !== id || item.quantity <= 1) return item;
    return { ...item, quantity: item.quantity - 1 };
  });
};

const increaseGuitarQuantity = (id) => {
  shoppingCart.value = shoppingCart.value.map((item) => {
    if (item.id !== id) return item;
    return { ...item, quantity: item.quantity + 1 };
  });
};

provide("emptyCart", emptyCart);
provide("removeGuitar", removeGuitarToCart);

watch(shoppingCart, () => trackingCart(), { deep: true });
</script>

<template>
  <Header
    :cart="shoppingCart"
    @decrease-quantity="decreaseGuitarQuantity"
    @increase-quantity="increaseGuitarQuantity"
  />
  <section class="main-banner">
    <div class="guitar-overview">
      <h1 class="guitar-overview-title">Modelo {{ guitar.name }}</h1>
      <p class="guitar-overview-desc">
        {{ guitar.description }}
      </p>
      <p class="guitar-overview-price">${{ guitar.price }}</p>
      <button
        class="guitar-overview-btncart"
        @click="() => addGuitarToCart(guitar)"
      >
        Agregar al carrito
      </button>
    </div>
    <img
      :src="`/img/${guitar.image}.png`"
      :alt="`Guitarra ${guitar.name}`"
      class="guitar_graphic"
    />
  </section>
  <main>
    <div class="guitar-grids">
      <Guitar
        v-for="guitar of guitars"
        :key="guitar.id"
        :guitar="guitar"
        @add-cart="addGuitarToCart"
        @show-overview="guitarOverview"
      />
    </div>
  </main>
</template>

<style>
main {
  max-width: 1320px;
  margin: auto;
}

.main-banner {
  display: flex;
  align-items: end;
  background-image: linear-gradient(
      90deg,
      rgba(0, 0, 0, 0.6),
      rgba(0, 0, 0, 0.6)
    ),
    url("img/header_bg_banner.jpg");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  justify-content: start;
  min-height: 520px;
  position: relative;
}

.guitar_graphic {
  position: absolute;
  width: 360px;
  right: 0;
  bottom: 0;
  pointer-events: none;
  z-index: 1;
}

.guitar-overview {
  max-width: 620px;
  padding: 3rem;
  position: relative;
  z-index: 10;
}
.guitar-overview-title {
  color: var(--color-primary);
  font-size: 4rem;
  margin: 0;
}
.guitar-overview-desc {
  color: var(--color-white);
}
.guitar-overview-price {
  color: var(--color-primary);
  font-size: 3rem;
  font-weight: 900;
  margin: 0;
}
.guitar-overview-btncart {
  all: unset;
  display: block;
  background-color: var(--color-primary);
  border-radius: 0.25rem;
  color: var(--color-white);
  font-size: 1.25rem;
  font-weight: 900;
  margin: 1.5rem 0 0 0;
  padding: 0.75rem 2rem;
}

.guitar-grids {
  display: grid;
  gap: 1.75rem;
  grid-template-columns: repeat(1, 1fr);
}

@media screen and (min-width: 768px) {
  .guitar-grids {
    grid-template-columns: repeat(
      auto-fill,
      minmax(0, calc((100% - (2 - 1) * 1.75rem) / 2))
    );
  }
}
@media screen and (min-width: 996px) {
  .guitar-grids {
    grid-template-columns: repeat(
      auto-fill,
      minmax(0, calc((100% - (3 - 1) * 1.75rem) / 3))
    );
  }
}
</style>
