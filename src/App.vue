<template>
  <div id="app">
    <b-container fluid>
      <h4 class="mb-4">Grid File Uploader</h4>

      <file-select
        v-model="file"
        :loading="file ? '파일 가져오는중...' : null"
      ></file-select>
    </b-container>
  </div>
</template>

<script>
import FileSelect from './components/FileSelect.vue';

export default {
  name: 'app',
  components: {
    FileSelect,
  },
  data() {
    return {
      file: null,
    };
  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 10px;
}
.border-thin {
  border-width: thin !important;
}
</style>
