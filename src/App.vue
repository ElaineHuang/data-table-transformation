<template>
  <div id="app">
    <div class="container">
      <h1 class="mb-4">Transform your legal document</h1>
      <form @submit.prevent="submitForm">
        <nav class="nav nav-pills nav-fill">
          <a class="nav-item nav-link" href="#" :class="{ active: tab === 0 }" @click="tab = 0">Upload files</a>
          <a class="nav-item nav-link" href="#" :class="{ active: tab === 1 }" @click="tab = 1">Input as string</a>
        </nav>
        <label v-if="tab === 0" class="file-block">
          <p class="mt-4">Please drop your .txt files here</p>
          <input type="file" accept=".txt" multiple @change="loadTextFromFile" />
        </label>
        <textarea v-if="tab === 1" v-model="currentData" class="form-control mt-3" cols="30" rows="10"></textarea>
        <button class="btn btn-primary submit mt-3" type="submit">Submit</button>
      </form>
      <div class="text-right">
        <button v-if="downloadable" class="btn btn-success submit mt-3" @click="download">Download Excel</button>
      </div>
    </div>
    <table class="table table-striped mt-5" v-if="tableData.length > 0">
      <thead class="thead-dark">
        <tr>
          <th v-for="(col, colIndex) in tableData[0]" :key="`${colIndex}`">{{ colIndex + 1 }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, rowIndex) in tableData" :key="`row-${rowIndex}`">
          <td v-for="(col, colIndex) in row" :key="`col-${colIndex}`">{{ col }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
//eslint-disable-next-line no-console 
import { export_json_to_excel } from './excel/Export2Excel'

export default {
  name: 'App',
  data () {
    return {
      currentData: '',
      tableData: [],
      downloadable: false,
      example: null,
      fileText: '',
      tab: 0
    }
  },
  methods: {
    submitForm () {
      const target = this.tab ? this.currentData : this.fileText
      const rows = target.trim().split('#')
      this.tableData = rows.map((row) => {
        const pureRow = row.trim()
        const cols = pureRow.split('!')
        return cols
      })
      this.downloadable = true
    },
    download () {
      const tHeader = []
      for (let i = 0; i < this.tableData[0].length; i++) {
        tHeader.push(i + 1)
      }
      export_json_to_excel({
        header: tHeader, //Header Required
        data: this.tableData, //Specific data Required
        filename: 'law-doc', //Optional
        autoWidth: true, //Optional
        bookType: 'xlsx' //Optional
      })
    },
    loadTextFromFile (e) {
      for (let i = 0; i < e.target.files.length; i++) {
        const file = e.target.files[i]
        const reader = new FileReader()
        reader.onload = e => {
          this.fileText = this.fileText + e.target.result
        }
        reader.readAsText(file, 'big5')
      }
    }
  }
}
</script>

<style>
@import "https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.submit {
  width: 150px;
}

.file-block {
  display: block;
  background: #eee;
  height: 220px;
  margin-top: 20px;
  border: 1px dashed #9a9a9a;
}
</style>
