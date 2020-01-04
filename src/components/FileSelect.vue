<template>
  <b-form-group
    label-cols="3"
    label-cols-sm="2"
    label-size="sm"
    label="파일 선택"
    label-for="selectFile"
    description="참고: xls, xlsx, csv 파일 포맷만 지원합니다."
  >
    <b-input-group>
      <b-form-file
        id="selectFile"
        class="col-sm-6 col-lg-5"
        size="sm"
        :value="value"
        :accept="acceptType"
        :disabled="Boolean(value)"
        @change="onFileChange($event)"
      ></b-form-file>

      <!-- input group append slot -->
      <template v-slot:append>
        <b-button
          size="sm"
          :disabled="!value || loading"
          @click="$emit('input', null)"
        >초기화</b-button>
      </template>
    </b-input-group>

    <!-- form group description slot -->
    <template v-slot:description v-if="loading && loadingText">
      <b-spinner class="border-thin" small></b-spinner>
      <span class="align-middle ml-1">{{ loadingText }}</span>
    </template>
  </b-form-group>
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
    loadingText: String,
  },
  data() {
    return {
      acceptType: fileAcceptTypes.join(','),
    };
  },
  methods: {
    onFileChange(e) {
      this.$emit('input', e.target.files[0] || null);
    },
  },
};
</script>

<style>

</style>
