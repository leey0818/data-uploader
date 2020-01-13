<template>
  <div>
    <v-file-input
      dense
      messages="참고: xls, xlsx, csv 파일 포맷만 지원합니다."
      :value="value"
      :accept="acceptType"
      :clearable="false"
      :disabled="Boolean(value)"
      :loading="loading"
      :placeholder="placeholder"
      @change="file => $emit('input', file)"
    ></v-file-input>
  </div>
</template>

<script>
const fileAcceptTypes = [
  'application/vnd.ms-excel',
  'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet',
  'text/csv',
  'text/plain',
];

export default {
  props: {
    value: File,
    loading: Boolean,
    placeholder: String,
  },
  data() {
    return {
      acceptType: fileAcceptTypes.join(','),
    };
  },
};
</script>
