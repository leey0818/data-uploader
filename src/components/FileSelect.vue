<template>
  <div>
    <v-file-input
      dense
      messages="참고: xls, xlsx, csv 파일 포맷만 지원합니다."
      prepend-inner-icon="$file"
      :accept="acceptType"
      :clearable="false"
      :disabled="loading"
      :error-messages="error ? `파일을 읽을 수 없습니다. 오류: ${error}` : null"
      :loading="loading"
      :placeholder="placeholder"
      :prepend-icon="null"
      :value="value"
      @change="file => $emit('input', file)"
    ></v-file-input>
    <div class="mt-3">
      <v-btn
        color="primary"
        depressed
        small
        :disabled="!value"
        @click="$emit('select')"
      >파일선택</v-btn>
    </div>
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
    error: String,
  },
  data() {
    return {
      acceptType: fileAcceptTypes.join(','),
    };
  },
};
</script>
