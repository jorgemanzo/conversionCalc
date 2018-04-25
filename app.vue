<template>
  <div>
    <center>
      <form id="inputForm" v-on:submit.prevent="convertInput()">
        <div class="row">
          <div class="col-md-4">
            <div class="radio">
              <input type="radio" name="datatype" value="Decimal" v-model="inputType" id="dec">
              <label for="dec">Decimal</label>
            </div>
          </div>
          <div class="col-md-4">
            <div class="radio">
              <input type="radio" name="datatype" value="Hexadecimal" v-model="inputType" id="hex">
              <label for="hex">Hexadecimal</label>
            </div>
          </div>
          <div class="col-md-4">
            <div class="radio">
                <input type="radio" name="datatype" value="Binary" v-model="inputType" id="bin">
                <label for="bin">Binary</label>
            </div>
          </div>
        </div>
        <h4>Your number to convert:</h4>
        <input type="text" v-model="rawInput" id="input"><br>
        <div class="checkbox">
          <input type="checkbox" id="fromNeg" v-model="checked">
          <label for="fromNeg">My input is negative, convert it to its negative equivalents! (please leave out the '-' from a negative decimal)</label>
        </div>
        <button class="btn btn-default" type="submit" id="convertButton">Convert</button>
      </form>
    </center>
    <center>
      <table>
        <tr>
          <th>Type</th>
          <th>Ouput</th>
        </tr>
        <tr>
          <td>Decimal</td>
          <td id="decOut">{{decimalOut}}</td>
        </tr>
        <tr>
          <td>Binary</td>
          <td id="binOut">{{binaryOut}}</td>
        </tr>
        <tr>
          <td>Hexadecimal</td>
          <td id="hexOut">{{hexadecimalOut}}</td>
        </tr>
      </table>
    </center>
  </div>
</template>

<script>
  import * as converter from './script.js';
  export default {
    data() {
      return  {
        checked: "",
        inputType: "",
        rawInput: "",
        decimalOut: "",
        hexadecimalOut: "",
        binaryOut: "",
        inputType: "",
        processed: 0
      }
    },
    methods:  {
      convertInput() {
        const USERINPUT = this.rawInput;
        console.log(this.inputType);
        if(this.checked){
          let complemented = 0;
          if(this.inputType === "Hexadecimal"){
            this.processed = converter.hexToBin(USERINPUT);
            complemented = converter.twosComplement(this.processed);
            this.decimalOut = converter.binToDec(complemented);
            this.binaryOut = complemented;
            this.hexadecimalOut = USERINPUT;
          } else if (this.inputType == "Decimal"){
            this.processed = converter.toBinary(USERINPUT);
            complemented = converter.twosComplement(this.processed);
            this.decimalOut = USERINPUT;
            this.binaryOut = complemented;
            this.hexadecimalOut = converter.toHex(converter.binToDec(complemented));
          } else {
            this.processed = USERINPUT;
            complemented = converter.twosComplement(this.processed);
            this.decimalOut = converter.binToDec(complemented);
            this.binaryOut = USERINPUT;
            this.hexadecimalOut = converter.toHex(converter.binToDec(complemented));
          }
        } else {
          if(this.inputType === "Hexadecimal"){
            this.processed = converter.hexToDec(USERINPUT);
          } else if(this.inputType == "Binary"){
            this.processed = converter.binToDec(USERINPUT);
          } else {
            this.processed = parseInt(USERINPUT);
          }
          this.decimalOut = this.processed;
          this.binaryOut = converter.toBinary(this.processed);
          this.hexadecimalOut = converter.toHex(this.processed);
        }
      }
    }
  }
</script>
