<template>
  <div id="calculator">
    <div class="display-pad">
      <div class="display-area">
        {{displayValue}}
      </div>
    </div>
    <div class="input-pad">
      <button class="key function-key" @click="displayValue !== '0' ? clearDisplay() : clearAll()">{{clearText}}</button>
      <button class="key function-key" @click="toggleSign()">±</button>
      <button class="key function-key" @click="inputPercent()">%</button>
      <button class="key operation-key" @click="performOperation('/')">÷</button>
      <button class="key" @click="inputDigit(7)">7</button>
      <button class="key" @click="inputDigit(8)">8</button>
      <button class="key" @click="inputDigit(9)">9</button>
      <button class="key operation-key" @click="performOperation('*')">x</button>
      <button class="key" @click="inputDigit(4)">4</button>
      <button class="key" @click="inputDigit(5)">5</button>
      <button class="key" @click="inputDigit(6)">6</button>
      <button class="key operation-key" @click="performOperation('-')">-</button>
      <button class="key" @click="inputDigit(1)">1</button>
      <button class="key" @click="inputDigit(2)">2</button>
      <button class="key" @click="inputDigit(3)">3</button>
      <button class="key operation-key" @click="performOperation('+')">+</button>
      <button class="key key-0" @click="inputDigit(0)">0</button>
      <button class="key" @click="inputDot()">.</button>
      <button class="key operation-key" @click="performOperation('=')">=</button>
    </div>
  </div>
</template>

<script type="text/javascript">
const CalculatorOperations = {
  '/' : function (preValue, nextValue) {
    return preValue / nextValue
  },
  '*' : function (preValue, nextValue) {
    return preValue * nextValue
  },
  '+' : function (preValue, nextValue) {
    return preValue + nextValue
  },
  '-' : function (preValue, nextValue) {
    return preValue - nextValue
  },
  '=' : function (preValue, nextValue) {
    return nextValue
  },
}
  export default {
    data () {
      return {
        value: null,
        displayValue: '0',
        operator: null,
        waitingForOperand: null
      }
    },
    methods: {
      // 输入数字
      inputDigit (digit) {
        if (this.waitingForOperand) {
          this.displayValue = String(digit)
        } else {
          this.displayValue = this.displayValue === '0' ? String(digit) : this.displayValue + digit
        }
      },
      // 输入点
      inputDot () {
        // 没有.时才能输入
        if (!(/\./).test(this.displayValue)) {
          this.displayValue += '.'
          this.waitingForOperand = false
        }
      },
      // 清除所有
      clearAll () {
        this.value = null
        this.displayValue = '0'
        this.operator = null
        this.waitingForOperand = false
      },
      // 清除展示
      clearDisplay () {
        this.displayValue = '0'
      },
      // 取反
      toggleSign () {
        let newValue = parseFloat(this.displayValue) * -1
        this.displayValue = String(newValue)
      },
      // 转为百分比
      inputPercent () {
        let currentValue = parseFloat(this.displayValue)
        if (currentValue === 0) {
          return
        }
        let fixedDigits = this.displayValue.replace(/^-?\d*\./, '')
        let newValue = parseFloat(this.displayValue) / 100
        this.displayValue = String(newValue.toFixed(fixedDigits.length + 2))
      },
      // 运算符操作
      performOperation (nextOperator) {
        let inputValue = parseFloat(this.displayValue)
        if (this.value === null) {
          this.value = inputValue
        } else if (this.operator) {
          let currentValue = this.value || 0
          let newValue = CalculatorOperations[this.operator](currentValue, inputValue)
          this.value = newValue
          this.displayValue = String(newValue)
        } 
        this.waitingForOperand = true
        this.operator = nextOperator
      }
    },
    computed: {
      clearText () {
        return this.displayValue !== '0' ? 'C' : 'AC'
      }
    }
  }
</script>

<style>
  html, body {
    margin: 0;
    height: 100%;
    overflow: hidden;
    font: 100 14px 'Robot';
  }
  #calculator {
    height: 100%;
    background: #000000;
  }
  #calculator .display-pad {
    height: 25%;
    width: 100%;
  }
  #calculator .display-pad .display-area {
    padding: 0 20px;
    float: right;
    font-size: 2rem;
    color: #FFFFFF;
  }
  #calculator .input-pad {
    height: 75%;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
  }
 #calculator .key {
    width: 100%;
    height: 100%;
    border-radius: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.5rem;
  }
  #calculator .key::after {
    border-radius: 0;
  }
  #calculator .key::active {
    box-shadow: inset 0 0 80px 0 rgba(0, 0, 0, 0.25);
  }
  #calculator .input-pad .key-0 {
    grid-column-start: 1;
    grid-column-end: 3;
  }
  #calculator .display-pad {
    background-color: #505050;
  }
  #calculator .function-key {
    background-color: #D4D4D2;
  }
  #calculator .operation-key {
    background-color: #FF9500;
    color: #FFFFFF;
    font-size: 0.7rem;
  }

</style>
