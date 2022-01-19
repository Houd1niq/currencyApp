<template>
  <div class="container">
    <header>
      <h1 class="header-title">
        {{ activeTab === "table" ? "Текущие курсы валют" : "Конвертер валют" }}
      </h1>
      <div class="menu">
        <a
          href="#"
          @click.prevent="activeTab = 'table'"
          :class="activeTab === 'table' ? 'active' : ''"
          class="menu-button"
          >Текущие курсы валют</a
        >
        <a
          href="#"
          @click.prevent="activeTab = 'converter'"
          :class="activeTab === 'converter' ? 'active' : ''"
          class="menu-button"
        >
          Конвертeр валют</a
        >
      </div>
    </header>
    <main>
      <table-app
        :data="this.currencyData"
        v-if="activeTab === 'table'"
      ></table-app>
      <converter-app
        :data="this.currencyData"
        v-if="activeTab === 'converter'"
      ></converter-app>
    </main>
  </div>
</template>

<script>
import TableApp from "@/components/TableApp";
import ConverterApp from "@/components/ConverterApp";
import axios from "axios";

export default {
  data() {
    return {
      activeTab: "table",
      currencyData: {},
    };
  },
  components: { ConverterApp, TableApp },
  name: "App",
  async beforeCreate() {
    const res = await axios.get("https://www.cbr-xml-daily.ru/daily_json.js");
    this.currencyData = res.data.Valute;
  },
};
</script>

<style lang="scss" scoped></style>
