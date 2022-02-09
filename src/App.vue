<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <div class="sudoku-table">
    <div class="sudoku-table-row" v-for="row in 9" :key="row">
      <div class="sudoku-table-cell" v-for="column in 9" :key="column">
        <!-- {{row}} x {{column}} -->
        <input v-model="numbers[row-1][column-1]" type="number">
        <div class="possible-numbers" v-if="!numbers[row-1][column-1]">
          <!-- {{possibleNumbers(row-1, column-1)}} -->
          <span 
          class="possible-number" 
          :class="{'possible-number-unique': isUnique(number, row-1, column-1)}" 
          v-for="number in availableNumber[row-1][column-1]" 
          :key="number">
            {{number}}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const possibleNumbers = [1,2,3,4,5,6,7,8,9];

export default {
  name: 'App',
  data() {
    return {
      numbers: []
    }
  },
  created() {
    for(let row=0; row<9; row++) {
      this.numbers[row] = [];
    }
    // this.numbers = [
    //   [5,3,null,null,7,null,null,null,null],
    //   [6,null,null,1,9,5,null,null,null],
    //   [null,9,8,null,null,null,null,6,null],
    //   [8,null,null,null,6,null,null,null,3],
    //   [4,null,null,8,null,3,null,null,1],
    //   [7,null,null,null,2,null,null,null,6],
    //   [null,6,null,null,null,null,2,8,null],
    //   [null,null,null,4,1,9,null,null,5],
    //   [null,null,null,null,8,null,null,7,9]
    // ]
  },
  computed: {
    availableNumber() {
      const allAvailableNumber = [];
      for (let row=0; row<9; row++) {
        allAvailableNumber[row] = [];
        for (let column=0; column<9; column++) {
          const possibleNumbers = this.possibleNumbers(row, column);
          allAvailableNumber[row][column] = possibleNumbers;
        }
      }
      return allAvailableNumber;
    }
  },
  methods: {
    isUnique(number, rowData, columnData) {
      let rowStart;
      let rowEnd;
      if (0 <= rowData && rowData <= 2) {
        rowStart = 0;
        rowEnd = 2;
      } else if(3 <= rowData && rowData <= 5) {
        rowStart = 3;
        rowEnd = 5;
      } else {
        rowStart = 6;
        rowEnd = 8;
      }
      let columnStart;
      let columnEnd;
      if (0 <= columnData && columnData <= 2) {
        columnStart = 0;
        columnEnd = 2;
      } else if(3 <= columnData && columnData <= 5) {
        columnStart = 3;
        columnEnd = 5;
      } else {
        columnStart = 6;
        columnEnd = 8;
      }
      
      for (let row=rowStart; row<=rowEnd; row++) {
        for (let column=columnStart; column<=columnEnd; column++) {
          if (row === rowData && column === columnData) continue;
          if (this.availableNumber[row][column].indexOf(number) !== -1) return false;
        }
      }
      return true
    },
    possibleNumbers(rowData, columnData) {
      const range = [];
      for (let row=0; row<9; row++) {
        for (let column=0; column<9; column++) {
          if (row == rowData || column == columnData) {
            if (this.numbers[row][column] && range.indexOf(this.numbers[row][column]) === -1) {
              range.push(this.numbers[row][column])
            }
          }
        }
      }
      let rowStart;
      let rowEnd;
      if (0 <= rowData && rowData <= 2) {
        rowStart = 0;
        rowEnd = 2;
      } else if(3 <= rowData && rowData <= 5) {
        rowStart = 3;
        rowEnd = 5;
      } else {
        rowStart = 6;
        rowEnd = 8;
      }
      let columnStart;
      let columnEnd;
      if (0 <= columnData && columnData <= 2) {
        columnStart = 0;
        columnEnd = 2;
      } else if(3 <= columnData && columnData <= 5) {
        columnStart = 3;
        columnEnd = 5;
      } else {
        columnStart = 6;
        columnEnd = 8;
      }
      
      for (let row=rowStart; row<=rowEnd; row++) {
        for (let column=columnStart; column<=columnEnd; column++) {
          if (this.numbers[row][column] && range.indexOf(this.numbers[row][column]) === -1) {
            range.push(this.numbers[row][column])
          }
        }
      }

      return possibleNumbers.filter(number => {
        return range.indexOf(number) === -1
      })
    }
  }
}
</script>

<style>
.sudoku-table {
  border: #000 solid 1px;
  border-collapse: collapse;
}

.sudoku-table-row {
  display:flex;
  justify-content: space-evenly;
}

.sudoku-table-cell {
  border: #999 solid 1px;
  border-collapse: collapse;
  width: calc(100% / 9);
  text-align: center;
  height: 100px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  position: relative;
}

.possible-numbers {
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  color: #999;
  width: 100%;
}

.possible-number-unique {
  color: #9b3535;
  font-weight: bold;
  font-size: 1.2em;
}

.possible-number:after {
    content: ",";
    font-weight: normal;
    font-size: 1em;
    color: #999;
}

.possible-number:last-child:after {
    content: ""
}

.sudoku-table-cell:first-child {
    border-left: #000 solid 2px;
}

.sudoku-table-cell:last-child {
    border-right: #000 solid 2px;
}

.sudoku-table-row:first-child .sudoku-table-cell {
  border-top: #000 solid 2px;
}

.sudoku-table-row:last-child .sudoku-table-cell {
  border-bottom: #000 solid 2px;
}

.sudoku-table-row:nth-child(6) .sudoku-table-cell, .sudoku-table-row:nth-child(3) .sudoku-table-cell {
  border-bottom: #000 solid 2px;
}

.sudoku-table-row:nth-child(7) .sudoku-table-cell, .sudoku-table-row:nth-child(4) .sudoku-table-cell {
  border-top: #000 solid 2px;
}

.sudoku-table-cell:nth-child(3), .sudoku-table-cell:nth-child(6) {
    border-right: #000 solid 2px;
}

.sudoku-table-cell:nth-child(4), .sudoku-table-cell:nth-child(7) {
    border-left: #000 solid 2px;
}

.sudoku-table-cell input {
    width: calc(100% - 5px);
    border: none;
    text-align: center;
    font-size: 1.5em;
}

.sudoku-table-cell input:focus-visible {
    outline: none;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type=number] {
  -moz-appearance: textfield;
}
</style>
