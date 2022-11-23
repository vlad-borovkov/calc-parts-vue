<template>
  <div class="main-table">
    <div class="maintable__header">
      <div class="maintable__header-item">Калькулятор для ремонта авто</div>
      <button @click="makePdf">Скачать в pdf</button>
      <button @click="handleAddBodyPart">Добавить деталь кузова</button>
    </div>
    <ul class="body-part">
      <BodyPart
        v-for="bodyPart in bodyPartArr"
        :bodyPart="bodyPart"
        @delete="handleDeleteBodyPart"
        @priceChange="handleEditBodyPartPrice"
        @countChange="handleEditBodyPartCount"
        @sumOfAllParts="setBodyPartTotalPrice"
      />
    </ul>
    <div class="main-table__ammount">
      <p class="ammount-title">Общая стоимость закупки</p>

      <money3
        class="ammount-value"
        v-model.number="sumValue.sumValue"
        v-bind="config"
        readonly="true"
      ></money3>
    </div>
  </div>
</template>

<script>
// импортируем компонент
import BodyPart from '../BodyPart/BodyPart.vue';
import { Money3Component } from 'v-money3';
export default {
  // регистрируем импортированные компоненты
  components: { BodyPart, money3: Money3Component },
  data() {
    return {
      sumValue: {
        sumValue: 0,
      },
      config: {
        masked: false,
        prefix: '',
        suffix: ' ₸',
        thousands: ',',
        decimal: '.',
        precision: 2,
        disableNegative: false,
        disabled: false,
        min: null,
        max: null,
        allowBlank: false,
        minimumNumberOfCharacters: 0,
      },
      bodyPartArr: [
        {
          id: Math.random(),
          bodyPartName: '',
          bodyPartPrice: '',
          bodyPartCount: 1,
          bodyPartTotalPrice: '',
        },
      ],
      partTotalPrice: '',
    };
  },
  methods: {
    handleAddBodyPart() {
      const newBodyPart = {
        id: Math.random(),
        bodyPartName: '',
        bodyPartPrice: '',
        bodyPartCount: 1,
        bodyPartTotalPrice: '',
      };

      this.bodyPartArr.push(newBodyPart);
    },

    makePdf() {
      this.$emit('makePdf');
    },

    // функция просчёта суммы всех ЧАСТЕЙ КУЗОВА для прокидывания в стейт выше
    calcSumAllBodyPart() {
      const sumOfAllBodyParts = this.bodyPartArr.reduce(
        (a, b) => a + b.bodyPartTotalPrice,
        0
      );
      this.sumValue.sumValue = sumOfAllBodyParts;
      // передаём просчитанную сумму всех ЧАСТЕЙ КУЗОВА
      //this.$emit('sumOfAllBodyParts', sumOfAllBodyParts);
    },

    //при изменении отдельной запчасти устанавливаем сумму всех запчастей внутри отдельного объекта массива bodyPartArr
    setBodyPartTotalPrice(bodyCardId, partTotalPrice) {
      this.bodyPartArr.map((item) => {
        if (item.id === bodyCardId) {
          item.bodyPartTotalPrice = partTotalPrice;
        }
      });
      this.calcSumAllBodyPart();
    },

    handleDeleteBodyPart(bodyCardItem) {
      const numberItemForDelete = this.bodyPartArr.indexOf(bodyCardItem);
      this.bodyPartArr.splice(numberItemForDelete, 1);

      this.calcSumAllBodyPart();
    },

    handleEditBodyPartPrice(bodyCardId) {
      this.bodyPartArr.map((item) => {
        if (item.id === bodyCardId) {
          item.bodyPartTotalPrice = item.bodyPartPrice * item.bodyPartCount;
        }
      });
      this.calcSumAllBodyPart();
    },

    handleEditBodyPartCount(bodyCardId, partsArrayLength, partTotalPrice) {
      this.bodyPartArr.map((item) => {
        if (item.id === bodyCardId && partsArrayLength === 0) {
          item.bodyPartTotalPrice = item.bodyPartPrice * item.bodyPartCount;
        } else if (item.id === bodyCardId && partsArrayLength >= 1) {
          // нужна сумма всех запчастей в карточке, а не тотал части кузова! прокинуть всю длинну масива и сумму всех запчастей
          const summAllParts = partTotalPrice;
          item.bodyPartTotalPrice = item.bodyPartCount * summAllParts;
        }
      });
      this.calcSumAllBodyPart();
    },
  },
};
</script>

<style scoped></style>
