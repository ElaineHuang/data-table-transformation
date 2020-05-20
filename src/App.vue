<template>
  <div id="app">
    <div class="container">
      <form @submit.prevent="submitForm">
        <textarea required v-model="currentData" class="form-control" cols="30" rows="10"></textarea>
        <button class="btn btn-primary submit mt-3" type="submit">Submit</button>
      </form>
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

export default {
  name: 'App',
  data () {
    return {
      currentData: '',
      tableData: []
    }
  },
  methods: {
    submitForm () {
      const rows = this.currentData.trim().split('#')
      this.tableData = rows.map((row) => {
        const pureRow = row.trim()
        const cols = pureRow.split('!')
        return cols
      })
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
</style>
