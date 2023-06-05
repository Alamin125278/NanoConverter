<script>
import "../src/assets/css/reset.css";
import "../src/assets/css/style.css";

function isBinary(value) {
  for (let t of value.toString()) {
    if (t !== "0" && t !== "1") return false;
  }
  return true;
}
function isOctal(value) {
  for (let t of value.toString()) {
    if (t > "0" && t < "7") return false;
  }
  return true;
}
function isHexadecimal(value) {
  for (let t of value.toString()) {
    if (t > "0" && t < "15") return false;
  }
  return true;
}
function textToBinary(text) {
  const charCodeArray = [];
  for (let i in text) {
    charCodeArray.push(text.charCodeAt(i));
  }
  const binaryArray = charCodeArray.map((char) => {
    return char.toString(2);
  });
  return binaryArray;
}
function binaryToText(codeArray) {
  let text = "";
  for (let code of codeArray) {
    const char = String.fromCharCode(parseInt(code, 2));
    text = text.concat(char);
  }
  return text;
}
export default {
  name: "App",
  data() {
    return {
      numbers: {
        decimal: 0,
        binary: 0,
        octal: 0,
        hexaDecimal: 0,
      },
      lines: {
        text: "",
        binary: "",
      },
      invalidNumber: false,
      invalidLine: false,
    };
  },
  methods: {
    numberReset(e) {
      e.preventDefault();
      [
        (this.numbers.decimal = 0),
        (this.numbers.binary = 0),
        (this.numbers.octal = 0),
        (this.numbers.hexaDecimal = 0),
      ];
    },
    resetLine(e) {
      e.preventDefault();
      [(this.lines.text = 0), (this.lines.binary = 0)];
    },
  },
  watch: {
    "numbers.decimal": function (value) {
      this.numbers.decimal = parseInt(value) || 0;
      this.numbers.binary = value.toString(2);
      this.numbers.octal = value.toString(8);
      this.numbers.hexaDecimal = value.toString(16);
    },
    "numbers.binary": function (value) {
      let decimal = parseInt(value, 2);
      if (!isBinary(value)) {
        this.invalidNumber = true;
      } else {
        this.invalidNumber = false;
      }
      this.numbers.decimal = decimal;
      this.numbers.binary = value || 0;
      this.numbers.octal = decimal.toString(8);
      this.numbers.hexaDecimal = decimal.toString(16);
    },
    "numbers.octal": function (value) {
      let decimal = parseInt(value, 8);
      if (!isOctal(value)) {
        this.invalidNumber = true;
      } else {
        this.invalidNumber = false;
      }
      this.numbers.decimal = decimal;
      this.numbers.binary = decimal.toString(2);
      this.numbers.octal = value || 0;
      this.numbers.hexaDecimal = decimal.toString(16);
    },
    "numbers.hexaDecimal": function (value) {
      let decimal = parseInt(value, 16);
      if (!isHexadecimal(value)) {
        this.invalidNumber = true;
      } else {
        this.invalidNumber = false;
      }
      this.numbers.decimal = decimal;
      this.numbers.binary = decimal.toString(2);
      this.numbers.octal = decimal.toString(8);
      this.numbers.hexaDecimal = value || 0;
    },
    "lines.text": function (value) {
      if (value.length == 0) {
        this.lines.binary = "";
      } else {
        const codeArray = textToBinary(value);
        this.lines.binary = codeArray.join(" ");
      }
    },
    "lines.binary": function (value) {
      if (value.length == 0) {
        this.lines.text = "";
      } else {
        const codeArray = value.split(" ");
        const text = binaryToText(codeArray);
        this.lines.text = text;
      }
    },
  },
};
</script>

<template>
  <div>
    <div class="container">
      <!-- Header section -->
      <header id="title">
        <h1>Nano Converter</h1>
      </header>
      <!-- Main Content section -->
      <main>
        <!-- Number system conversion -->
        <section id="number-system">
          <h2>Number System Conversion</h2>
          <div class="box">
            <div class="input-group">
              <label for="">Decimal</label>
              <input type="number" v-model="numbers.decimal" />
            </div>
            <div class="input-group">
              <label for="">Binary</label>
              <input type="number" v-model="numbers.binary" />
            </div>
            <div class="input-group">
              <label for="">Octal</label>
              <input type="number" v-model="numbers.octal" />
            </div>
            <div class="input-group">
              <label for="">HexaDecimal</label>
              <input type="text" v-model="numbers.hexaDecimal" />
            </div>
            <div class="reset-btn">
              <p v-if="invalidNumber" style="color: red">Invalid input</p>
              <button @click="numberReset">Reset</button>
            </div>
            <div class="one-rem-space"></div>
          </div>
        </section>
        <!-- Text conversion -->
        <section id="text-conversion">
          <h2>Text Conversion</h2>
          <div class="box">
            <div class="input-group">
              <label for="">Enter Your Text Below</label>
              <textarea
                v-model="lines.text"
                name="text-field"
                cols="30"
                rows="10"
              ></textarea>
            </div>
            <div class="half-rem-margin"></div>
            <div class="input-group">
              <label for="">Enter Your Binary Below</label>
              <textarea
                v-model="lines.binary"
                name="binary-field"
                cols="30"
                rows="10"
              ></textarea>
            </div>
            <div class="reset-btn">
              <button @click="resetLine">Reset</button>
            </div>
            <div class="one-rem-space"></div>
          </div>
        </section>
      </main>
    </div>
  </div>
</template>

<style scoped></style>
