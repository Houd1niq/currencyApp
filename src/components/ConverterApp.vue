<template>
  <div class="converter">
    <div class="converter_from converter_block">
      <h3 class="converter-title">Ковертируемая валюта</h3>
      <div class="converter-nav">
        <div class="selected_cur">{{ convertingCur }}</div>
        <a @click="dropdownHandler($refs.converting)" class="chose_cur" href="#"
          >Выберите валюту</a
        >
      </div>
      <ul ref="converting" class="converter-dropdown hide">
        <li
          @click="choseCurHandler('converting', item, $refs.converting)"
          v-for="item in data"
          :key="item"
        >
          <span class="cur_name">{{ item.Name }}</span
          ><span class="cur_char">{{ item.CharCode }}</span>
        </li>
        <li @click="rubHandler('converting', $refs.converting)">
          <span class="cur_name">Рубль</span><span class="cur_char">RUR</span>
        </li>
      </ul>
      <input
        class="base_cur"
        type="number"
        ref="startInput"
        @input="startConverting($event.target)"
      />
    </div>

    <img class="arrow-svg" src="../assets/transfer.svg" alt="arrwos" />

    <div class="converter_to converter_block">
      <h3 class="converter-title">Конвертированная валюта</h3>
      <div class="converter-nav">
        <div class="selected_cur">{{ convertedCur }}</div>
        <a @click="dropdownHandler($refs.converted)" class="chose_cur" href="#"
          >Выберите валюту</a
        >
      </div>
      <ul ref="converted" class="converter-dropdown hide">
        <li
          @click="choseCurHandler('converted', item, $refs.converted)"
          v-for="item in data"
          :key="item"
        >
          <span class="cur_name">{{ item.Name }}</span
          ><span class="cur_char">{{ item.CharCode }}</span>
        </li>
        <li @click="rubHandler('converted', $refs.converted)">
          <span class="cur_name">Рубль</span><span class="cur_char">RUR</span>
        </li>
      </ul>
      <input
        class="base_cur"
        ref="endInput"
        @input="endConverting($event.target)"
        type="number"
      />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    data: Object,
  },
  data() {
    return {
      startInputValue: 0,
      endInputValue: 0,
      convertingCur: "RUR",
      convertedCur: "-",
      startCurValue: 1,
      endCurValue: 0,
    };
  },
  methods: {
    dropdownHandler(menu) {
      menu.classList.toggle("hide");
    },
    choseCurHandler(type, cur, menu) {
      if (type === "converting") {
        this.convertingCur = cur.CharCode;
        this.startCurValue = cur.Value;
        this.startConverting(this.$refs.startInput);
      } else if (type === "converted") {
        this.convertedCur = cur.CharCode;
        this.endCurValue = cur.Value;
        this.startConverting(this.$refs.startInput);
      }
      menu.classList.toggle("hide");
    },
    startConverting(input) {
      if (this.endCurValue !== 0 && input.value >= 0) {
        this.startInputValue = Number(input.value);
        this.$refs.endInput.value =
          (this.startInputValue * this.startCurValue) / this.endCurValue;
      }
    },
    endConverting(input) {
      if (input.value >= 0) {
        this.endInputValue = Number(input.value);
        this.$refs.startInput.value =
          (this.endInputValue * this.endCurValue) / this.startCurValue;
      }
    },
    rubHandler(type, menu) {
      if (type === "converting") {
        this.convertingCur = "RUR";
        this.startCurValue = 1;
        this.startConverting(this.$refs.startInput);
      } else if (type === "converted") {
        this.convertedCur = "RUR";
        this.endCurValue = 1;
        this.startConverting(this.$refs.startInput);
      }
      menu.classList.toggle("hide");
    },
  },
  name: "ConverterApp",
};
</script>

<style scoped></style>
