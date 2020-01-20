<template>
  <v-app>
    <v-content>
      <v-container fluid>
        <h2>파일 업로드</h2>

        <v-stepper v-model="step" vertical>
          <v-stepper-step :complete="step > 1" step="1">파일 선택</v-stepper-step>
          <v-stepper-content step="1">
            <file-select
              placeholder="파일을 선택하세요."
              v-model="file"
              :loading="loading"
              :error="errorMessage"
            ></file-select>
          </v-stepper-content>

          <v-stepper-step :complete="step > 2" step="2">
            시트 선택 <small v-if="step > 2">"{{ currentSheetName }}" 선택됨</small>
          </v-stepper-step>
          <v-stepper-content step="2">
            <sheet-select v-model="currentSheetName" :items="sheetNames"></sheet-select>
            <v-btn
              color="primary"
              depressed
              small
              :disabled="currentSheetName === null"
              @click="step = 3"
            >선택완료</v-btn>
            <v-btn class="ml-1" depressed small @click="file = null">이전으로</v-btn>
          </v-stepper-content>

          <v-stepper-step step="3">매핑할 데이터 선택</v-stepper-step>
          <v-stepper-content step="3">
            <!-- todo -->
          </v-stepper-content>

          <v-stepper-step step="4">데이터 확인</v-stepper-step>
          <v-stepper-content step="4">
            <!-- todo -->
          </v-stepper-content>
        </v-stepper>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import XLSX from 'xlsx';
import FileSelect from './components/FileSelect.vue';
import SheetSelect from './components/SheetSelect.vue';

let workbook;

export default {
  name: 'app',
  components: {
    FileSelect,
    SheetSelect,
  },
  data() {
    return {
      step: 1,
      file: null,
      loading: false,
      errorMessage: null,
      currentSheetName: null,
      sheetNames: [],
      headers: [],
      items: [],
    };
  },
  watch: {
    file(v) {
      if (v === null) {
        this.step = 1;
        this.workbook = null;
        this.currentSheetName = null;
        this.sheetNames = [];
      } else {
        // this.step = 2;
        this.readFile(v);
      }
    },

    currentSheetName(v) {
      if (v === null) {
        this.headers = [];
        this.items = [];
      } else {
        const sheet = this.getSheet(v);
        if (sheet) {
          const maxCol = sheet.map(o => o.length).reduce((p, c) => Math.max(p, c));
          this.headers = Array(maxCol).fill({});
          this.items = sheet;
        }
      }
    },
  },
  methods: {
    readFile(file) {
      // read start
      this.loading = true;
      this.errorMessage = null;

      const readFile = f => new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.onerror = evt => reject(evt.target.error);
        reader.onload = evt => resolve(new Uint8Array(evt.target.result));
        reader.readAsArrayBuffer(f);
      });

      const parseFile = data => new Promise((resolve, reject) => {
        try {
          const wb = XLSX.read(data, { type: 'array' });
          resolve(wb);
        } catch (e) {
          reject(e);
        }
      });

      readFile(file).then(parseFile).then((wb) => {
        this.step = 2;
        this.loading = false;
        this.sheetNames.push(...wb.SheetNames);

        workbook = wb;

        if (this.sheetNames.length === 1) {
          this.currentSheetName = this.sheetNames[0];
        }
      }).catch((e) => {
        this.loading = false;
        this.errorMessage = e.message;
      });
    },

    getSheet(name) {
      if (workbook && workbook.Sheets[name]) {
        return XLSX.utils.sheet_to_json(workbook.Sheets[name], { header: 1 });
      }
      return null;
    },
  },
};
</script>
