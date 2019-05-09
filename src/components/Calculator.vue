<template>
  <div class="calculator">
    <div class="input">{{ current || 0}}</div>
    <div @click="clear" class="button actions-top">{{ clearButton || 'C' }}</div>
    <div @click="sign" class="button actions-top">+/-</div>
    <div @click="percent" class="button actions-top">%</div>
    <div @click="addOperator('/')" class="button actions-right">/</div>
    <div @click="append('7')" class="button ">7</div>
    <div @click="append('8')" class="button">8</div>
    <div @click="append('9')" class="button">9</div>
    <div @click="addOperator('*')" class="button actions-right">*</div>
    <div @click="append('4')" class="button">4</div>
    <div @click="append('5')" class="button">5</div>
    <div @click="append('6')" class="button">6</div>
    <div @click="addOperator('-')" class="button actions-right">-</div>
    <div @click="append('1')" class="button">1</div>
    <div @click="append('2')" class="button">2</div>
    <div @click="append('3')" class="button">3</div>
    <div @click="addOperator('+')" class="button actions-right">+</div>
    <div @click="append('0')" class="button zero">0</div>
    <div @click="dot" class="button">.</div>
    <div @click="result" class="button actions-right">=</div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      before: '',
      clearButton: 'AC',
      current: '',
      operator: '',
      operatorClicked: false,
    }
  },
  name: 'Calculator',
  methods: {
    clear () {
      this.current = '';
      this.before = '';
      this.clearButton = 'AC';
      this.operator = '';
    },
    sign () {
      if (this.current.length === 0) {
        this.current = "-0"
      } else if (this.current.charAt(0) === '-') {
          this.current = this.current.substr(1)
      } else {
          this.current = `-${this.current}`
      }
    },
    percent () {
      this.current = `${parseFloat(this.current) / 100}`
    },
    dot () {
      if (this.current.length === 0) {
        this.append('0.')
      } else if (this.current.indexOf('.') === -1) {
        this.append('.')
      }
    },
    append (number) {
      if (this.current === "-0") {
        this.current = this.current.substr(2);
        this.current = `-${this.current}`;
      } else if (this.operatorClicked === true){
        this.operatorClicked = false;
        this.current = ''
      }
      this.current = `${this.current}${number}`;
      this.clearButton = ''
    },
    result () {
      this.current = eval(`${this.before}${this.operator}${this.current}`);
      this.current = this.current.toString();
    },
    addOperator (number) {
      if (this.operator !== '') {
        this.operator = '';
        this.operator = `${this.operator}${number}`
      } else if (this.operator.length === 0){
        this.before = this.current;
        this.operator = `${this.operator}${number}`
      }
      this.operatorClicked = true;
      this.before = this.current
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .calculator {
    width: 450px;
    height : 760px;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    font-size: 3.2em;
    border: 4px solid #595959;
    border-radius: 10px;
    line-height: 2.3em;
  }
  .input {
    grid-column: 1/5;
    background-color: #595959;
    color: white;
    text-align: right;
    padding: 40px 20px 0 0;
    font-size: 1.5em;
  }
  .zero {
    grid-column: 1/3;
  }
  .button {
    background-color: #818181;
    border: 1px solid #595959;
  }
  .actions-top {
    background-color: #686868;
    color: white;
  }
  .actions-right {
    background-color: #F0A147;
    color: white;
  }
</style>
