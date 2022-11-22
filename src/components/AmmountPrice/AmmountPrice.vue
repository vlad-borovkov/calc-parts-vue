<template>
  <div class="ammount-price">
    <p class="ammount-price__title">Общая стоимость закупки</p>

    <p class="ammount-price__price">
      {{ sumValue.sumValue }}
    </p>
  </div>
</template>

<script>
export default {
  props: {
    sumValue: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped></style>
