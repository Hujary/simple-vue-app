<template>
    <div> 
      <h1> Barcode App</h1>
      <StreamBarcodeReader @decode="onDecode" @loaded="onLoaded" id="readerID"></StreamBarcodeReader>
    </div>

    <div id="div2ID">
      <button id="ButtonID" > Recognized </button>
      <p id="ScannDataID" style="padding-left: 30px"> Information: {{ ScannData }}</p>
    </div>

    <div id="ProduktFensterID" v-if="visible==true"> 
      <div > 
        <img id="ProductPictureID" :src="Datenbank[this.ScannData].src" /> 
      </div>
      <div> 
        {{ Datenbank[123456789012].name }}
      </div>
    </div>
</template>

<script>
import { StreamBarcodeReader } from "vue-barcode-reader";
export default {
  data() {
    return {
      ScannData: "text",
      visible: false,
      Datenbank: { 
        4059549000152: {
          name: "Atla",
          Qualität: "für alle Tafeln geeignet",
          Größe: "72 Stück", 
          src: "https://m.media-amazon.com/images/W/IMAGERENDERING_521856-T1/images/I/61MypS1KawL._AC_SS450_.jpg"
        },
        123456789012: {
          name: "Test", 
          Qualität: "Gut",
          Größe: "Leer", 
          src: "https://www.hellabrunn.de/fileadmin/_processed_/c/f/csm_tierpark-hellabrunn-wasserschwein-capybara-amerika-tierlexikon-artenschutz_7c6c6695e6.png"
        }
      }
    }
  },
  components: {
    StreamBarcodeReader
  },
   methods: {
    onDecode (result) { 
    this.ScannData = result; 

      if(result == "4059549000152"){
        console.log("Scan war erfolgreich: " + result);
        this.visible=true;
        document.getElementById("ButtonID").style.background='#008000';
      }  

      if(result == "123456789012"){
        console.log("Scan war erfolgreich: " + result);
        this.visible=true;
        document.getElementById("ButtonID").style.background='#008000';
      } 

    /*for(let i=0; i<Datenbank.length(); i++){
      console.log("funktioniert");
    } */
    
    }
  } 
}
</script>

<style >
#readerID {
  background: brown;
}
#ProductPictureID {
  width: 150px;
  height: 150px;
}
#ButtonID{
  text-align: left;
  color: rgb(0, 0, 0);
}
#ProduktFensterID {
  display: flex;
  flex-direction: row;
  padding-top: 20px;
}
#div2ID {
  display: flex;
  flex-direction: row
}
</style>
