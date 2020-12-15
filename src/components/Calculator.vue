<template>
  <div class="calculator">
    <Display :value="expression" />
    <Button label="C" @onClick="cleanDisplay" />
    <Button label="(" @onClick="clickParentheses" />
    <Button label=")" @onClick="clickParentheses" />
    <Button label="<=" @onClick="backSpace" />
    <Button label="1" @onClick="clickNumber" />
    <Button label="2" @onClick="clickNumber" />
    <Button label="3" @onClick="clickNumber" />
    <Button label="+" @onClick="clickOperation" />
    <Button label="4" @onClick="clickNumber" />
    <Button label="5" @onClick="clickNumber" />
    <Button label="6" @onClick="clickNumber" />
    <Button label="-" @onClick="clickOperation" />
    <Button label="7" @onClick="clickNumber" />
    <Button label="8" @onClick="clickNumber" />
    <Button label="9" @onClick="clickNumber" />
    <Button label="*" @onClick="clickOperation" />
    <Button label="0" @onClick="clickNumber" />
    <Button label="." @onClick="clickDot" />
    <Button label="/" @onClick="clickOperation" />
    <Button label="=" @onClick="calculate" />
  </div>
</template>

<script>
import Button from "./template/Button";
import Display from "./template/Display";
export default {
  name: "Calculator",
  components: { Button, Display },
  data: function () {
    return {
      expression: "0",
      dot: false,
      parentheses: false,
      parenthesesLeft: false,
    };
  },
  methods: {
    clickKeyboard(e){
      switch(e.key){
        case "Escape": 
          this.cleanDisplay();
          break;
        case "Backspace": 
          this.backSpace();
          break;
        case "(":
        case ")":
          this.clickParentheses(e.key);
          break;
        case "Enter":
          this.calculate();
          break;
        case ".":
          this.clickDot(); 
          break;
        case "/":
        case "+":
        case "*":
        case "-":
          this.clickOperation(e.key);
          break;
        default:
          this.clickNumber(e.key);
      }
    },

    backSpace(){
      if (this.expression.length == 1) this.expression = "0";
      else this.expression = this.expression.substr(0, this.expression.length - 1);
    },

    cleanDisplay() {
      this.expression = 0;
      this.parentheses = false;
      this.parenthesesLeft = false;
      this.dot = false;
    },

    clickParentheses(n) {
      const lastChar = this.expression[this.expression.length - 1];
      var isOp = this.lastIsOperation();

      if (!this.parentheses) {
        if (!this.parenthesesLeft) {
          if (n === "(") {
            if (this.expression === "0") {
              this.expression = "(";
              this.parentheses = true;
              this.parenthesesLeft = true;
            } else {
              if (lastChar === ".") {
                this.expression = this.expression.substr(
                  0,
                  this.expression.length - 1
                );
                this.expression += "*(";
                this.parentheses = true;
                this.parenthesesLeft = true;
              } else if (!isOp) {
                this.expression += "*(";
                this.parentheses = true;
                this.parenthesesLeft = true;
              } else {
                this.expression += "(";
                this.parentheses = true;
                this.parenthesesLeft = true;
              }
            }
          }
        }
      } else if (n === ")") {
        if (lastChar === ".") {
          this.expression += "0*)";
          this.parentheses = false;
          this.parenthesesLeft = false;
        } else if (!isOp) {
          this.expression += ")";
          this.parentheses = false;
          this.parenthesesLeft = false;
        }
      }
    },

    calculate() {
      this.expression = eval(this.expression);
    },

    clickNumber(n) {
      const val = parseFloat(n);
      const lastChar = this.expression[this.expression.length - 1];

      if (!isNaN(val)) {
        if (lastChar == ")") this.expression += "*" + val;
        else if (this.expression == "0") this.expression = val.toString();
        else this.expression += val;
      }
    },

    clickDot() {
      if (!this.dot) {
        const isOp = this.lastIsOperation();
        if (!isOp) {
          this.expression += ".";
        }
        this.dot = true;
      }
    },

    clickOperation(n) {
      //when be operation
      var isOp = this.lastIsOperation();
      const lastChar = this.expression[this.expression.length - 1];
      if (isOp) {
        this.expression = this.expression.substr(0, this.expression.length - 1);
        this.expression += n;
      } else if (lastChar == ".") {
        this.expression = this.expression.substr(0, this.expression.length - 1);
        this.expression += n;
      } else if (lastChar != "(") {
        this.expression += n;
      }
      this.dot = false;
    },

    lastIsOperation() {
      const lastChar = this.expression[this.expression.length - 1];
      if (
        lastChar == "+" ||
        lastChar == "-" ||
        lastChar == "*" ||
        lastChar == "/"
      ) {
        return true;
      } else {
        return false;
      }
    },
  },
  mounted(){
    window.onkeydown = (e)=> this.clickKeyboard(e);
  }
}
</script>

<style>
.calculator {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;

}
</style>