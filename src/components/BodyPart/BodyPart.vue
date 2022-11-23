<template>
  <li class="body-part__item">
    <div class="body-part__field">
      <div class="body-part__input-wrap">
        <label class="body-part__header-item">Деталь кузова</label>
        <input
          type="text"
          class="body-part__name"
          placeholder="Введите название"
          v-model="bodyPart.bodyPartName"
        />
      </div>
      <div
        v-bind:class="[
          this.partsArray.length === 0
            ? `body-part__input-wrap`
            : `body-part__price_off`,
        ]"
        class="body-part__input-wrap"
      >
        <label class="body-part__header-item">Цена</label>
        <input
          type="number"
          class="body-part__price"
          min="0"
          placeholder="Введите цену"
          v-model="bodyPart.bodyPartPrice"
          @input="handleEditBodyPartPrice"
        />
      </div>

      <div class="body-part__input-wrap">
        <label class="body-part__header-item">Количество</label>
        <input
          type="number"
          class="body-part__count"
          min="1"
          v-model="bodyPart.bodyPartCount"
          @input="handleEditBodyPartCount"
        />
      </div>
      <div class="body-part__input-wrap">
        <label class="body-part__header-item">Общая стоимость</label>
        <textarea
          class="body-part__total-price"
          type="text"
          readOnly
          cols="10"
          rows="1"
          placeholder="Итого"
          v-model="bodyPart.bodyPartTotalPrice"
        ></textarea>
      </div>

      <div class="body-part__actions">
        <p class="body-part__header-item">Действия</p>
        <button @click="handleDeleteBodyPart" class="body-part_btn">
          Удалить деталь
        </button>
        <button @click="handleAddSinglePart" class="body-part_btn">
          Добавить запчасть
        </button>
      </div>
    </div>
    <!-- условный рендерринг группы элементов -->
    <template v-if="partsArray.length >= 1">
      <fieldset class="single-part__field">
        <legend>Запчасти:</legend>
        <ul class="single-part">
          <SinglePart
            v-for="singlePart in partsArray"
            :singlePart="singlePart"
            @deleteSinglePart="handleDeleteSinglePart"
            @changePartPrice="handleEditSinglePartPrice"
            @changePartCount="handleEditSinglePartCount"
          />
        </ul>
      </fieldset>
    </template>
  </li>
</template>

<script>
import SinglePart from '../SinglePart/SinglePart.vue';
export default {
  components: { SinglePart },
  //привязываем каждый объект итерации к основному массиву
  props: {
    bodyPart: {
      type: Object,
      required: true,
    },
  },

  data() {
    return {
      partsArray: [],
    };
  },

  methods: {
    handleAddSinglePart() {
      const newSinglPart = {
        id: Math.random(),
        singlePartName: '',
        singlePartPrice: '',
        singlePartCount: 1,
        singlePartTotalPrice: '',
      };

      this.partsArray.push(newSinglPart);
    },

    calcSumAllSingleParts() {
      const sumOfAllParts = this.partsArray.reduce(
        (a, b) => a + b.singlePartTotalPrice,
        0
      );
      // передаём сумму всех ЗАПЧАСТЕЙ внутри ЧАСТИ кузова

      this.$emit('sumOfAllParts', this.bodyPart.id, sumOfAllParts);
    },

    handleDeleteSinglePart(singlePartCard) {
      const numberItemForDelete = this.partsArray.indexOf(singlePartCard);
      this.partsArray.splice(numberItemForDelete, 1);

      this.calcSumAllSingleParts();
    },
    handleEditSinglePartPrice(singlePartId) {
      this.partsArray.map((item) => {
        if (item.id === singlePartId) {
          item.singlePartTotalPrice =
            item.singlePartPrice * item.singlePartCount;
        }
      });
      this.calcSumAllSingleParts();
    },

    handleEditSinglePartCount(singlePartId) {
      this.partsArray.map((item) => {
        if (item.id === singlePartId) {
          item.singlePartTotalPrice =
            item.singlePartCount * item.singlePartPrice;
        }
      });
      this.calcSumAllSingleParts();
    },
    // прокидываем объект текущей bodyPart наверх
    handleDeleteBodyPart() {
      this.$emit('delete', this.bodyPart);
    },
    handleEditBodyPartPrice(e) {
      //console.log(e.target.value);
      this.$emit('priceChange', this.bodyPart.id);
    },
    handleEditBodyPartCount(e) {
      const sumOfAllParts = this.partsArray.reduce(
        (a, b) => a + b.singlePartTotalPrice,
        0
      );
      // передаём сумму всех ЗАПЧАСТЕЙ внутри ЧАСТИ кузова

      this.$emit(
        'countChange',
        this.bodyPart.id,
        this.partsArray.length,
        sumOfAllParts
      );
    },
  },
};
</script>

<style scoped></style>
