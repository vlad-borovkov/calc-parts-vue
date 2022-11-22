<template>
  <div class="main-table">
    <div class="maintable__header">
      <div class="maintable__header-item">Калькулятор для ремонта авто</div>
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
  </div>
</template>

<script>
// импортируем компонент
import BodyPart from '../BodyPart/BodyPart.vue';
export default {
  // регистрируем импортированные компоненты
  components: { BodyPart },
  data() {
    return {
      bodyPartArr: [
        {
          id: Math.random(),
          bodyPartName: 'Тестовое',
          bodyPartPrice: '',
          bodyPartCount: 1,
          bodyPartTotalPrice: '',
        },
      ],
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

    // функция просчёта суммы всех ЧАСТЕЙ КУЗОВА для прокидывания в стейт выше
    calcSumAllBodyPart() {
      const sumOfAllBodyParts = this.bodyPartArr.reduce(
        (a, b) => a + b.bodyPartTotalPrice,
        0
      );
      // передаём просчитанную сумму всех ЧАСТЕЙ КУЗОВА
      this.$emit('sumOfAllBodyParts', sumOfAllBodyParts);
    },

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

    handleEditBodyPartCount(bodyCardId, partsArrayLength) {
      this.bodyPartArr.map((item) => {
        if (item.id === bodyCardId) {
          item.bodyPartTotalPrice = item.bodyPartPrice * item.bodyPartCount;
        } else if (item.id === bodyCardId && partsArrayLength >= 1) {
          // нужна сумма всех запчастей в карточке, а не тотал части кузова! прокинуть всю длинну масива
          item.bodyPartCount = newBodyPartCount;
          item.bodyPartTotalPrice = totalBodyPrice * item.bodyPartCount;
        }
      });
      this.calcSumAllBodyPart();
    },
  },
};
</script>

<style scoped></style>
