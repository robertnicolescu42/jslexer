<template>
  <div class="home">
  <v-container>
  <h1>Please select your file:</h1>
  <input type="file" ref="myFile" @change="selectedFile">
  <h3 class="mt-6">Or:</h3>
    <v-container fluid>
      <v-textarea
        ref="writtenText"
        clearable
        clear-icon="mdi-close-circle"
        label="Paste your code here"
        auto-grow
        value=""
      ></v-textarea>
  </v-container>
    <v-btn
    block
    elevation="2"
    large
    outlined
    onclick=""
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

          /*
          for(i = 0; i < code.length; i++){
            ch = code[i];
            console.log(ch);
            if(/[a-zA-Z]/.test(ch)){
              continue;
            }

            if(code[i] == "\n") {
              continue;
              }
              
          }*/
         /*
          for (i = 0; i < code.length; i++) {
              ch = code[i];
              //console.log("code[" , i , "]= " , code[i] , "\n");
              if (/\s+/.test(ch)) {
                  continue;
              } //gets rid of whitespace
              console.log("code[", i, "]= ", ch, "\n");
              if(/[a-zA-Z]/.test(ch)){ //is a character
                console.log(ch , "is a letter");
                buffer = "";
                while(/[a-zA-Z0-9]/.test(ch)){
                  console.log(ch , " is either a letter or a number");
                  break;
                }
              }
            }
           }
           */
           //console.log(splitElem);


export default {
  name: 'Home',
  components: {

  },
  methods:{
    selectedFile() {
      //console.log('selected a file');
      //console.log(this.$refs.myFile.files[0]);
      
      let file = this.$refs.myFile.files[0];
      if(!file || file.type !== 'text/plain') return;
      
      let reader = new FileReader();
      reader.readAsText(file, "UTF-8");
      reader.onload =  evt => {
        let text = evt.target.result;
        
        //console.log(text);
        this.lexer(text.toString());
      }
      reader.onerror = evt => {
        console.error(evt);
      }
    },
    lexer(code){
      let operators = ['=','+','-',';','>','<','<>',':','*', "++", "--", "!", "+=", "-=","{","}","(",")"];
      console.log(code);
      let splitElem = code.split(/[,?\s+]/); //^[_a-z]\\w*$
      let arr = [];
      let codes = [];
      /*
      split.forEach(element => {
        if(element == "") {split.splice(element);}   
      });

      for (var i = splitElem.length - 1; i >= 0; i--) {
        if (splitElem[i] == "") {
          splitElem.splice(i, 1);
        }}

      console.log(splitElem);

      splitElem.forEach(element => {

        if(operators.includes(element)){ //is element
          Symb(element, "operator", arr, codes);
        }
        console.log("current symbol: " + element);
        if(/[0-9]+/.test(element)){
          //is number (doesn't work)
          console.log("number: " + element);
          Symb(element, "number", arr, codes);
        }
      });*/
      let i;
      let buffer = "";
      var ch;
      for(i = 0; i <= code.length; i++){
        ch = code[i];
        if(/\s+/.test(ch)){
          continue;
        } //gets rid of whitespace

        if(/[a-zA-z]/.test(ch)){
          buffer = "";
          while(/[a-zA-z0-9]/.test(ch)){
            buffer += ch;
            //console.log("identifier: " + ch);
            ch = code[i+1];
            i++;
          }
        }

      //console.log(arr);
      //console.log(codes);
    }
    }

  }
}
</script>

