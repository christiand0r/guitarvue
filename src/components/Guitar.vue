<script setup>
import { computed } from "vue";

const props = defineProps({
  guitar: {
    type: Object,
    required: true,
  },
});

const emits = defineEmits(["add-cart", "show-overview"]);

const getExcerpt = (text) => {
  text = text.trim();

  if (text === "") return "";

  const length = text.split("").length;

  return length <= 100 ? text : text.slice(0, 96) + "...";
};
</script>

<template>
  <figure class="guitar-card" @click="$emit('show-overview', guitar)">
    <picture class="guitar-picture">
      <img
        :src="`/img/${guitar.image}.jpg`"
        :alt="`Imagen de ${guitar.image}`"
      />
    </picture>
    <div>
      <figcaption class="guitar-caption">
        <h3 class="guitar-name">{{ guitar.name }}</h3>
        <p class="guitar-desc">{{ getExcerpt(guitar.description) }}</p>
        <p class="guitar-price">${{ guitar.price }}</p>
      </figcaption>
      <button
        type="button"
        class="guitar-button"
        @click.stop="$emit('add-cart', guitar)"
      >
        Agregar al carrito
      </button>
    </div>
  </figure>
</template>

<style scoped>
.guitar-card {
  display: flex;
  gap: 1rem;
  margin: 0;
  padding: 2rem 1rem;
  width: 100%;
  cursor: pointer;
}

.guitar-picture {
  aspect-ratio: 9 / 16;
  min-width: 120px;
  max-width: 180px;
}
.guitar-picture img {
  display: block;
  height: 100%;
  object-fit: contain;
  width: 100%;
}

.guitar-desc {
  margin: 0.75rem 0;
}

.guitar-name {
  font-size: 1.5rem;
  font-weight: 700;
  margin: 0;
  text-transform: uppercase;
}
.guitar-price {
  color: var(--color-primary);
  font-size: 1.75rem;
  font-weight: 900;
  margin: 0 0 1rem;
}

.guitar-button {
  background-color: var(--color-gray-900);
  border-radius: 0.25rem;
  color: var(--color-white);
  font-weight: 900;
  padding: 0.75rem 2rem;
  text-transform: uppercase;
  font-size: 0.75rem;
}
@media screen and (min-width: 992px) {
  .guitar-button {
    font-size: 1rem;
  }
}
</style>
