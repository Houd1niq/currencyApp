<template>
  <div id="dd" class="wrapper-dropdown">
    <span @click="showDropdown">Выберите основную валюту</span>
    <ul ref="dropdown" class="dropdown">
      <li @click="dropdownHandlerRub()">
        <a href="#">RUR</a>
      </li>
      <li @click="dropdownHandler(item)" v-for="item in data" :key="item">
        <a href="#">{{ item.CharCode }}</a>
      </li>
    </ul>
  </div>
  <div class="table-wrapper">
    <h3 class="table-title">
      Курсы валют по сравнению с
      <span style="font-weight: 400">{{ currentCurrency }}</span>
    </h3>
    <table cellspacing="0" cellpadding="0">
      <thead>
        <tr>
          <th>Полное наименование</th>
          <th>Валюта</th>
          <th>Курс сегодня</th>
          <th>Курс вчреа</th>
          <th>Изменение</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in filtredObject" :key="item">
          <td>{{ item.Name }}</td>
          <td>{{ item.CharCode }}</td>
          <td>{{ currencyToday(item).toFixed(4) }}</td>
          <td>{{ currencyYesterday(item).toFixed(4) }}</td>
          <td>{{ calculateDif(item) }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  props: {
    data: Object,
  },
  data() {
    return {
      currentCurrency: "RUR",
      baseNominal: 1,
    };
  },
  name: "TableApp",
  methods: {
    calculateDif(item) {
      return `${(item.Value / (item.Previous / 100) - 100).toFixed(4)}%`;
    },
    showDropdown() {
      this.$refs.dropdown.classList.toggle("shown");
    },
    dropdownHandler(item) {
      this.currentCurrency = item.CharCode;
      this.baseNominal = item.Value;
      this.$refs.dropdown.classList.toggle("shown");
    },
    dropdownHandlerRub() {
      this.currentCurrency = "RUR";
      this.baseNominal = 1;
      this.$refs.dropdown.classList.toggle("shown");
    },
    currencyToday(item) {
      return item.Value / this.baseNominal;
    },
    currencyYesterday(item) {
      return item.Previous / this.baseNominal;
    },
  },
  computed: {
    filtredObject() {
      return Object.values(this.data).filter((item) => {
        return item.CharCode !== this.currentCurrency;
      });
    },
  },
};
</script>

<style scoped></style>
