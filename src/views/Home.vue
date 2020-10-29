<template>
  <div class="home">
  <v-container>
  <h1>Please select your file:</h1>
  <input  type="file" ref="myFile" @change="selectedFile">
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
    v-on:click.native="lexer()"
    >Run
    </v-btn>
  </v-container>
  </div>
</template>

<script>




 function Symb(Sy, Code, arr, codes){
      if (!arr.includes(Sy)){
        arr.push(Sy);
        codes.push(Code);
        /*
        return arr.findIndex(Sy.toString());
        */
      }/*
      else{
        return arr.findIndex(Sy.toString());
      }*/
    }

var operators = ['++', '--', '=', '+', '-', '>', '<', '<>', '*', '!', '+=', '-=', '>=', '<=', '=='];
var signs = ['{', '}', '(', ')' ,';',',', ':'];
var reserved = ['void', 'int', 'if', 'else','for', 'while'];
var ch;
var arr = [];
var codes = [];
var code;
var text;
var results = [];

    function analyze(item){
        let trueval = item[0];

        if(operators.includes(trueval)){
          results.push([trueval, "(OP) operator"]);
        }

        else if(/^(\d+)$/.test(trueval)){
          results.push([trueval, "(NO) number"]);
        }

        else if(reserved.includes(trueval)){
          results.push([trueval, "(KW) reserved keyword"]);
        }

        else if(signs.includes(trueval)){
          results.push([trueval, "(SP) punctuation mark"]);
        }
        
        else if (/^[a-zA-Z_$][a-zA-Z_$0-9]*$/.test(trueval)){
          results.push([trueval, "(ID) identifier"]);
        }

        else{
          results.push([trueval, "(ERR) Unknown symbol"]);
        }
    }

    function lexer() {
      code = document.getElementById('textarea').value = text.toString();
      console.log("Initial code:\n" + code);
      //let splitElem = code.split(/[\s+]/); //^[_a-z]\\w*$
      //let splitElem = code.split(/[\w]/); //^[_a-z]\\w*$
      let i;
      let buffer = "";

      var regexp = /(\w+|\+\+|--|=|<|>|\(|\)|\{|\}|>=|<=|==|;|\*|\/)/g;

      var array = [...code.matchAll(regexp)];
      console.log(array);

      array.forEach(analyze);

      console.table(results);


      }
// @ is an alias to /src


export default {
  name: 'Home',
  components: {

  },
  methods:{
    selectedFile(event) {
      //console.log('selected a file');
      //console.log(this.$refs.myFile.files[0]);
      let file = this.$refs.myFile.files[0];
      if (!file || file.type !== 'text/plain') return;

      let reader = new FileReader();
      reader.readAsText(file, "UTF-8");
      reader.onload = evt => {
          text = evt.target.result;

          //console.log(text);
          lexer(text.toString());
          document.getElementById('textarea').value = text.toString();
      }
      reader.onerror = evt => {
          console.error(evt);
      }

      },


    }
}
</script>