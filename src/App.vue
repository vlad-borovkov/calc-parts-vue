<template>
  <div class="page">
    <MainTable @makePdf="makePDF" />
  </div>
</template>

<script>
// импортируем фалы компонентов
import MainTable from './components/MainTable/MainTable.vue';
import html2PDF from 'jspdf-html2canvas';

export default {
  // регистрируем импортированные компоненты
  components: { MainTable },
  data() {
    return {
      sumBodyPartValue: {
        sumValue: '',
      },
    };
  },
  methods: {
    makePDF() {
      let page = document.getElementsByClassName('main-table');

      html2PDF(page, {
        jsPDF: {
          format: 'a4',
        },
        margin: { left: 10, right: 10, top: 10, bottom: 10 },
        imageType: 'image/jpeg',
        output: './pdf/generate.pdf',
      });
    },
  },
};
</script>

<style></style>
