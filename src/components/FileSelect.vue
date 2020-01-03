<template>
  <b-form-group
    label-cols="2"
    label-size="sm"
    label="파일 선택"
    label-for="selectFile"
    :description="value ? null : '참고: xls, xlsx, csv 파일 포맷만 지원합니다.'"
  >
    <b-form-file
      id="selectFile"
      class="col-sm-7"
      size="sm"
      :value="value"
      :accept="acceptType"
      :disabled="Boolean(value)"
      @change="onFileChange($event)"
    ></b-form-file>

    <!-- form group description slot -->
    <template slot="description" v-if="loading">
      <b-spinner class="border-thin" small></b-spinner>
      <span class="align-middle ml-1">{{ loading }}</span>
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
    loading: String,
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
