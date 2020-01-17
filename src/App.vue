<template>
  <div id="app">
    <div  id="header">
      <h1>Simple Binary - Hex - Decimal Converter</h1>
    </div>


    <div id="content">
      <div id="entry_boxes">
        <div class="flex_holder">
          <h2>Decimal</h2>
          <input class="input_box" v-model="decimal" @input="convertDecimal">
        </div>

        <div class="flex_holder">
          <h2>Hex</h2>
          <input class="input_box" v-model="hex" @input="convertHex">
        </div>

        <div class="flex_holder">
          <h2>Binary</h2>
          <input class="input_box" v-model="binary" @input="convertBinary">
        </div>
      </div>

      <div id="history_container">
        <div id="history_banner">
          <h2>History</h2>
          <button id="clear_button" @click="clearHistory">Clear</button>
        </div>
        <div id="history_boxes_container">
          <div id="history_boxes">
            <history v-for="n in n_history"
                     v-bind:key="n.id"
                     v-bind:value="n">
            </history>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import history from "@/components/history";
import _ from 'lodash';

export default {
  name: 'app',
  data: function() {
    return {
      decimal: null,
      hex: null,
      binary: null,
      n_history: []
    };
  },
  components: {
    history
  },
  methods:{
    clearHistory: function(){
      this.n_history = [];
    },
    addHistory: function(){
      this.n_history.unshift(
              {id: this.n_history.length,
              hex: this.hex,
              decimal: this.decimal,
              binary: this.binary}
      )
    },
    convertDecimal: _.debounce(function () {

      let num = parseInt(this.decimal, 10);
      if(isNaN(num)){
        this.hex = null;
        this.binary = null;
      } else {
        this.hex = "0x" + num.toString(16).toUpperCase();
        this.binary = "0" + num.toString(2);
        this.addHistory();
      }
    }, 500),

    convertHex: _.debounce(function () {

      let num = this.hex.slice(0, this.hex.length);

      if (num.startsWith("0x" || "0X")){
        num = num.slice(2);
      }

      num = parseInt(num, 16);
      if(isNaN(num)){
        this.decimal = null;
        this.binary = null;
      } else {
        this.decimal = num;
        this.binary = "0" + num.toString(2);
        this.addHistory();
      }
    }, 500),
    
    convertBinary: _.debounce(function () {

      let num = this.binary.slice(0, this.binary.length);
      num = parseInt(num, 2);

      if(isNaN(num)){
        this.decimal = null;
        this.hex = null;
      } else {
        this.decimal = num;
        this.hex = "0x" + num.toString(16).toUpperCase();
        this.addHistory();
      }
    }, 500)
  }
}
</script>

<style>
/*
blue: #1E305B
light-blue: #80A4ED
*/
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #1E305B;
}

#header{
  width: 100%;
  text-align: center;
}

#content{
  display: flex;
  flex-wrap: wrap;
}

#entry_boxes{
  flex: 1 360px;
  padding: 0 5% 5% 5%;
  display: flex;
  flex-direction: column;
  align-content: space-between;
}

.flex_holder{
  padding-bottom: 5%;
}

.flex_holder:last-child{
  padding-bottom: 0;
}

.input_box{
  width: 100%;
  height: 6vh;
  border: #1E305B 2px solid;
  border-radius: 5px;
  padding-left: 2%;
  font-size: 2em;
}

.input_box:focus{
  border: lightblue 2px solid;
  transition: 0.5s;
}

#history_container{
  flex: 1 360px;
  padding: 0 5% 5% 5%;
  display: flex;
  flex-direction: column;
}

#history_banner{
  display: flex;
  align-content: space-between;
  align-items: center;
  flex: 0 1 auto;
}

#history_banner > h2{
  flex: 1 1 auto;
}

#history_boxes_container{
  flex: 1 0 0;
  overflow-y: auto;
}

#history_boxes_container::-webkit-scrollbar {
  width: 5px;
}

/* Track */
#history_boxes_container::-webkit-scrollbar-track {
  box-shadow: inset 0 0 5px grey;
  border-radius: 2px;

}

/* Handle */
#history_boxes_container::-webkit-scrollbar-thumb {
  background: #1E305B;
  border-radius: 2px;
}

/* Handle on hover */
#history_boxes_container::-webkit-scrollbar-thumb:hover {
  background: lightblue;
}

#history_boxes{
  width: 100%;
  display: grid;
  grid-template-columns: 1fr;
  row-gap: 5px;
}

#clear_button{
  background-color: #1E305B;
  color: white;
  border: none;
  border-radius: 5px;
  height: 30px;
  width: 60px;
  transition: 0.5s;
}

#clear_button:hover{
  background-color: white;
  color: #1E305B;
  border: lightblue 2px solid;
}

h1{
  font-size: 3em;
}

h2{
  font-size: 2em;
  color: #1E305B;
}

</style>
