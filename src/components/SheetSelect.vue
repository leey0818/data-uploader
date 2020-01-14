<template>
  <div>
    <v-select
      dense
      :value="value"
      :items="sheetItems"
      @input="item => $emit('input', item)"
    ></v-select>
  </div>
</template>

<script>
export default {
  props: {
    value: String,
    items: {
      type: Array,
      validator(value) {
        return value.filter(v => typeof v === 'string').length === value.length;
      },
    },
  },
  data() {
    return {
      blankItem: {
        value: null,
        text: '데이터를 가져올 시트를 선택하세요',
      },
    };
  },
  computed: {
    sheetItems() {
      return [this.blankItem, ...this.items];
    },
  },
};
</script>
