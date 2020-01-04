<template>
  <div id="app">
    <b-container fluid>
      <h4 class="mb-4">Grid File Uploader</h4>

      <file-select
        v-model="file"
        :loading="loading"
        loadingText="파일 읽어오는중..."
      ></file-select>

      <b-form-group
        label-cols="3"
        label-cols-sm="2"
        label-size="sm"
        label="시트 선택"
        v-if="sheetNames.length > 1"
      >
        <b-form-select
          class="col-sm-6 col-lg-5"
          size="sm"
          v-model="currentSheetName"
          :options="sheetNames"
        ></b-form-select>
      </b-form-group>
    </b-container>
  </div>
</template>

<script>
import XLSX from 'xlsx';
import FileSelect from './components/FileSelect.vue';

let workbook;

export default {
  name: 'app',
  components: {
    FileSelect,
  },
  data() {
    return {
      file: null,
      loading: false,
      currentSheetName: null,
      sheetNames: [],
    };
  },
  watch: {
    file(v) {
      if (v === null) {
        this.workbook = null;
        this.currentSheetName = null;
        this.sheetNames = [];
      } else {
        this.readFile(v);
      }
    },
  },
  methods: {
    readFile(file) {
      // read start
      this.loading = true;

      const reader = new FileReader();
      reader.onload = (e) => {
        const data = new Uint8Array(e.target.result);

        // read file
        workbook = XLSX.read(data, { type: 'array' });
        this.loading = false;
        this.sheetNames = workbook.SheetNames;
        this.currentSheetName = this.sheetNames[0] || null;

        const firstSheet = workbook.Sheets[this.currentSheetName];
        const sheetData = XLSX.utils.sheet_to_json(firstSheet, { header: 1 });
        const maxCol = sheetData.map(v => v.length).reduce((p, c) => Math.max(p, c));

        console.log(workbook);
        console.log(maxCol);
        console.log(sheetData);
      };
      reader.onprogress = (e) => {
        this.percent = (e.total / e.loaded).toFixed(2);
      };

      reader.readAsArrayBuffer(file);
    },
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
