<template>
  <li class="single-part__item">
    <div class="single-part__input-wrap">
      <label>Запчасть</label>
      <input
        type="text"
        class="single-part__name"
        v-model="singlePart.singlePartName"
        placeholder="Введите название"
      />
    </div>
    <div class="single-part__input-wrap">
      <label>Цена</label>
      <input
        type="number"
        class="single-part__price"
        v-model="singlePart.singlePartPrice"
        @input="handleEditSinglePartPrice"
        min="0"
        placeholder="Введите цену"
      />
    </div>
    <div class="single-part__input-wrap">
      <label>Кол-во</label>
      <input
        type="number"
        class="single-part__count"
        placeholder="Шт"
        min="0"
        v-model="singlePart.singlePartCount"
        @input="handleEditSinglePartCount"
      />
    </div>
    <div class="single-part__input-wrap">
      <label>Общая стоимость</label>
      <textarea
        class="single-part__total-price"
        type="number"
        placeholder="Итого"
        v-model="singlePart.singlePartTotalPrice"
        readOnly
        cols="{10}"
        rows="{1}"
      ></textarea>
    </div>

    <div class="single-part__actions">
      <button @click="handleDeleteSinglePart">Удалить</button>
    </div>
  </li>
</template>

<script>
export default {
  props: {
    singlePart: {
      type: Object,
      required: true,
    },
  },
  methods: {
    handleDeleteSinglePart() {
      this.$emit('deleteSinglePart', this.singlePart);
    },
    handleEditSinglePartPrice() {
      this.$emit('changePartPrice', this.singlePart.id);
    },
    handleEditSinglePartCount() {
      this.$emit('changePartCount', this.singlePart.id);
    },
  },
};
</script>

<style scoped></style>
