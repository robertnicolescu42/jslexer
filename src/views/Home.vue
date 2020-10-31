<template>
  <div class="home">
  <v-container>
  <h1>Please select your file:</h1>
  <!-- <input  type="file" ref="myFile" @change="selectedFile"> -->
    <v-file-input
    accept="text/*"
    label="File input"
    @change = "selectedFile"
  ></v-file-input>
  <h3 class="mt-6"></h3>
    <v-container fluid>
      <v-textarea
        ref="writtenText"
        clearable
        clear-icon="mdi-close-circle"
        label=""
        auto-grow
        value=""
        id = "textarea"
        rows = "15"
      ></v-textarea>
  </v-container>
  <v-btn
    block
    elevation="2"
    large
    outlined
    v-on:click.native="lexer(code)"
    >Run
    </v-btn>
    <p>
      {{ code }}
    </p>
<!--
    <v-simple-table dense>
      <thead>
        <tr>
          <th class="text-left">
            Token
          </th>
          <th class="text-left">
            Category
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="item in desserts"
          :key="item.name"
        >
          <td>{{ item.name }}</td>
          <td>{{ item.calories }}</td>
        </tr>
      </tbody>
  </v-simple-table>
-->
  </v-container> 

  </div>
</template>

<script>
var operators = ['++', '--', '=', '+', '-', '>', '<', '<>', '*', '!', '+=', '-=', '>=', '<=', '=='];
var signs = ['{', '}', '(', ')' ,';',',', ':'];
var reserved = ['void', 'int', 'if', 'else','for', 'while'];
var code;
var text;
var results = [];
// @ is an alias to /src

export default {
  name: 'Home',
  data () {
    return {
      code : '',
      result : [
        { token: '' },
        { category: '' }
      ]
    }
  },
  components: {

  },
methods: {
    lexer: function(code) {
      if (code == ""){
        code = document.getElementById('textarea').value;
      }
        //console.log("Initial code:\n" + code);
        //let splitElem = code.split(/[\s+]/); //^[_a-z]\\w*$
        //let splitElem = code.split(/[\w]/); //^[_a-z]\\w*$
        let i;
        let buffer = "";

        var regexp = /(\w+|\+\+|--|=|<|>|\(|\)|\{|\}|>=|<=|==|;|\*|\/)/g;

        var array = [...code.matchAll(regexp)];
        //console.log(array);

        array.forEach(this.analyze);

        console.table(results);

        //this.results = results;
    },

    analyze: function(item) {
        let trueval = item[0];

        if (operators.includes(trueval)) {
            results.push([trueval, "(OP) operator"]);
        } else if (/^(\d+)$/.test(trueval)) {
            results.push([trueval, "(NO) number"]);
        } else if (reserved.includes(trueval)) {
            results.push([trueval, "(KW) reserved keyword"]);
        } else if (signs.includes(trueval)) {
            results.push([trueval, "(SP) punctuation mark"]);
        } else if (/^[a-zA-Z_$][a-zA-Z_$0-9]*$/.test(trueval)) {
            results.push([trueval, "(ID) identifier"]);
        } else {
            results.push([trueval, "(ERR) Unknown symbol"]);
        }
    },
    
    selectedFile(event) {
        //console.log(event);

        let reader = new FileReader();
        reader.readAsText(event);

        reader.onload = evt => {
            text = evt.target.result;

            //console.log(text);
            this.code = text.toString();
        }
        reader.onerror = evt => {
            console.error(evt);
        }

    }

}
}
</script>