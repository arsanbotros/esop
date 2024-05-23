<script>
export default {
  data: function () {
    return {
      yearsCount: 7,
      startingYear: 2023,
      grantedOptionsYear: [3000, 3750, 3900, 4050, 4200],
    };
  },
  computed: {
    years: function () {
      let yearsList = [];
      for (let x = 0; x < this.yearsCount; x++) {
        yearsList.push(Number(this.startingYear) + Number(x));
      }
      return yearsList;
    },
  },
  methods: {
    getCellValue: function (row, column) {
      if (row <= column && column - row < 3) {
        if (!this.grantedOptionsYear[row]) {
          return "";
        }

        return Math.floor(this.grantedOptionsYear[row] / 3);
      } else {
        return "";
      }
    },
    getYearAvailable: function (column) {
      let sum = 0;

      for (var count = 0; count <= this.yearsCount; count++) {
        sum += Number(this.getCellValue(count, column));
      }

      return sum;
    },
  },
};
</script>

<template>
  <table>
    <tr>
      <td class="header"></td>
      <td class="header" v-for="(year, index) in years" :key="index">
        Y{{ index + 1 }} ({{ year }})
      </td>
    </tr>

    <tr v-for="(year, indexrow) in years" :key="indexrow + 'r'">
      <td class="header">Y{{ indexrow + 1 }} ({{ year }})</td>
      <td v-for="(number, indexcolumn) in years" :key="indexcolumn + 'yc'">
        {{ getCellValue(indexrow, indexcolumn) }}
      </td>
    </tr>
    <tr>
      <td class="aggregations">
        Total Excersieable options <br />(if previous not exercised)
      </td>
      <td class="aggregations" v-for="(year, index) in years" :key="index">
        {{ getYearAvailable(index) }}
      </td>
    </tr>
  </table>

  <hr />

  <div>
    <label for="yearsCount">Years count</label>
    <input id="yearsCount" v-model="yearsCount" />
  </div>
  <div>
    <label for="startingYear">staring year</label>
    <input type="number" id="startingYear" v-model="startingYear" />
  </div>

  <hr />

  <div v-for="(number, index) in years">
    <label for="grantedOptionsYear">Total Granted for {{ number }}</label>
    <input
      type="number"
      id="startingYear"
      v-model="grantedOptionsYear[index]"
    />
  </div>
</template>

<style scoped>
table {
  font-weight: bold;
  border-collapse: collapse;
  width: 100%;
}
table,
tr,
td {
  border: 1px solid #000;
}
.header {
  background: #008800;
  color: #fff;
  width: 100px !important;
}
.aggregations {
  background: #880000;
  color: #fff;
}
</style>
