<script>
  import ButtonInput from './components/ButtonInput.vue'
  import NavigatorMenu from './components/NavigatorMenu.vue'
  import CalculateDisplay from './components/CalculateDisplay.vue'
  export default {
    components: { NavigatorMenu, ButtonInput, CalculateDisplay },
    name: 'App',
    data() {
      return {
        histroryOperation: [],
        currentCalculate: '0',
        lastOperation: '0'
      }
    },
    methods: {
      getPercent() {
        let percent = this.currentCalculate.replace(',', '.') / 100
        this.currentCalculate = percent.toString().replace('.', ',')
      },
      allClear() {
        this.currentCalculate = '0'
        this.lastOperation = '0'
      },
      doDelete() {
        console.log(this.currentCalculate)
        if (this.currentCalculate !== '0') {
          this.currentCalculate = this.currentCalculate.toString().slice(0, -1)
        }
        if (this.currentCalculate === '') {
          this.currentCalculate = '0'
        }
      },
      inputValue(val) {
        if (val === '0' || val === '00') {
          if (this.currentCalculate === '0') {
            this.currentCalculate = '0'
          } else {
            this.currentCalculate += this.maskedIn(val)
          }
        } else if (val === ',') {
          if (this.currentCalculate.includes(',')) {
            this.currentCalculate = this.currentCalculate
          } else {
            if (this.lastIsOperand()) {
              return this.currentCalculate
            } else {
              this.currentCalculate += this.maskedIn(val)
            }
          }
        } else if (val === '/' || val === '*' || val === '-' || val === '+') {
          if (this.lastIsOperand()) {
            let str = this.currentCalculate.toString()
            let vars = str.slice(0, -1) + val
            this.currentCalculate = this.maskedIn(vars)
          } else {
            this.currentCalculate += this.maskedIn(val)
          }
        } else {
          if (this.currentCalculate == '0' || this.currentCalculate === 'NaN') {
            this.currentCalculate = this.maskedIn(val)
          } else {
            this.currentCalculate += this.maskedIn(val)
          }
        }
        this.$nextTick(function () {
          this.scrollRight()
        })
      },
      scrollRight() {
        let scrollbar = document.getElementById('numberInput')
        scrollbar.scrollLeft = scrollbar.scrollWidth
      },
      scrolldown() {
        let scrollbar = document.getElementById('resultElement')
        scrollbar.scrollTop = scrollbar.scrollHeight
      },
      calculate() {
        if (this.lastIsOperand()) {
          let str = this.currentCalculate.toString()
          let vars = str.slice(0, -1)
          this.currentCalculate = vars
          if (this.checkContainsOperand(this.currentCalculate)) {
            this.counted()
          } else {
            return this.currentCalculate
          }
        } else {
          this.counted()
        }
        this.$nextTick(function () {
          this.scrolldown()
        })
      },
      counted() {
        let rep = this.currentCalculate.replace('X', '*').replace(',', '.')
        let lastCalculate = this.currentCalculate
        let currentCalt = eval(rep).toString()
        this.addHistroy(lastCalculate, currentCalt.replace('.', ','))
        this.currentCalculate = currentCalt.replace('.', ',')
      },
      maskedIn(val) {
        return val.toString().replace('*', 'X')
      },
      lastIsOperand() {
        let operand = this.currentCalculate.slice(-1)
        if (
          operand === '+' ||
          operand === '-' ||
          operand === 'X' ||
          operand === '/'
        ) {
          return true
        } else {
          return false
        }
      },
      checkContainsOperand(calculateString) {
        const operand = ['+', '-', 'X', '/']
        let str = calculateString
        if (operand.some((v) => str.includes(v))) {
          return true
        } else {
          return false
        }
      },
      addHistroy(operation, result) {
        if (this.checkContainsOperand(this.currentCalculate)) {
          this.histroryOperation.push({
            operation: operation,
            result: result
          })
        }
      },
      deleteHistory() {
        this.histroryOperation = []
      }
    }
  }
</script>
<template>
  <div class="h-screen w-full bg-slate-800 p-0 md:p-2">
  <div class="main-container">
    <navigator-menu :deleteHistory="deleteHistory" />
    <calculate-display :deleteHistory="deleteHistory" :histroryOperation="histroryOperation" :currentCalculate="currentCalculate" :lastOperation="lastOperation" />
    <div class="button-wrapper">
      <div class="flex h-full w-full justify-between space-x-2 text-gray-300">
        <button-input class="btn-primary" symbols="AC" @click="allClear()" />
        <button-input class="btn-primary" symbols="DEL" @click="doDelete()" />
        <button-input class="btn-primary" symbols="%" @click="getPercent()" />
        <button-input class="btn-primary" symbols="/" @click="inputValue('/')" />
      </div>
      <div class="flex h-full w-full justify-between space-x-2 text-white">
        <button-input class="btn-primary" symbols="7" @click="inputValue(7)" />
        <button-input class="btn-primary" symbols="8" @click="inputValue(8)" />
        <button-input class="btn-primary" symbols="9" @click="inputValue(9)" />
        <button-input class="btn-primary" symbols="X" @click="inputValue('*')" />
      </div>
      <div class="flex h-full w-full justify-between space-x-2 text-white">
        <button-input class="btn-primary" symbols="4" @click="inputValue(4)" />
        <button-input class="btn-primary" symbols="5" @click="inputValue(5)" />
        <button-input class="btn-primary" symbols="6" @click="inputValue(6)" />
        <button-input class="btn-primary" symbols="-" @click="inputValue('-')" />
      </div>
      <div class="flex h-full w-full justify-between space-x-2 text-white">
        <button-input class="btn-primary" symbols="1" @click="inputValue(1)" />
        <button-input class="btn-primary" symbols="2" @click="inputValue(2)" />
        <button-input class="btn-primary" symbols="3" @click="inputValue(3)" />
        <button-input class="btn-primary" symbols="+" @click="inputValue('+')" />
      </div>
      <div class="flex h-full w-full justify-between space-x-2 text-white">
        <button-input class="btn-primary" symbols="00" @click="inputValue('00')" />
        <button-input class="btn-primary" symbols="0" @click="inputValue('0')" />
        <button-input class="btn-primary" symbols="," @click="inputValue(',')" />
        <button-input class="btn-secondary" symbols="=" @click="calculate()" />
      </div>
    </div>
  </div>
</div>

</template>

<style scoped lang="postcss">
  .main-container {
    @apply m-auto w-full md:w-2/4 px-2 lg:w-1/4 rounded-none md:rounded-md ring-1 ring-blue-900 bg-slate-900 h-full flex flex-col;
  }
  .button-wrapper {
    @apply flex-1 py-2 flex flex-col space-y-2 text-3xl font-bold select-none;
  }
  .btn-primary {
    @apply bg-slate-800 hover:bg-slate-700 hover:text-blue-500;
  }

  .btn-secondary {
    @apply bg-blue-700 hover:bg-blue-800  hover:text-white;
  }
</style>
