<template>
  <div class="page">
    <MainTable @sumOfAllBodyParts="sumOfAllBodyParts" />
    <AmmountPrice :sumValue="sumBodyPartValue" />
  </div>
</template>

<script>
// импортируем фалы компонентов
import MainTable from './components/MainTable/MainTable.vue';
import AmmountPrice from './components/AmmountPrice/AmmountPrice.vue';

export default {
  // регистрируем импортированные компоненты
  components: { MainTable, AmmountPrice },
  data() {
    return {
      sumBodyPartValue: {
        sumValue: '',
      },
    };
  },
  methods: {
    sumOfAllBodyParts(sumValue) {
      this.sumBodyPartValue.sumValue = sumValue;
    },
  },
};
</script>

<style>
/* можно добавить scoped */
/* флаг scoped говорит о том, что файл стилей будет доступен только для этого компонента и не доступен другим извне */
</style>
