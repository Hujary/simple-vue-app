<template>
    <div> 
      <h1> Barcode App</h1>
      <StreamBarcodeReader @decode="onDecode" @loaded="onLoaded" id="readerID"></StreamBarcodeReader>
    </div>

    <div id="div2ID">
      <button id="ButtonID"> Recognized </button>
      <p id="ScannDataID" style="padding-left: 30px"> Information: {{ ScannData }}</p>
    </div>

    <div id="ProduktFensterID" v-if="visible==true"> 
      <div > 
        <img id="ProductPictureID" :src="PrintObjekt.src" /> 
      </div>
      <div> 
        {{ PrintObjekt.name }}
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
      PrintObjekt: {
        Name: "empty", Qualität: "empty", Größe: "empty", src: "empty"
      },
      Datenbank: { 
        4059549000152: {
          name: "Atla", Qualität: "für alle Tafeln geeignet", Größe: "72 Stück", BildURL: "https://m.media-amazon.com/images/W/IMAGERENDERING_521856-T1/images/I/61MypS1KawL._AC_SS450_.jpg"
       },
       50000112630299: {
          name: "Fuze", Größe: "1L", BildURL: "https://www.worldofsweets.de/out/pictures/master/product/1/fuzetea-schwarzer-tee-pfirsich-400ml-no1-4837.jpg"
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
        this.visible=true;
        document.getElementById("ButtonID").style.background='#008000';
        this.PrintObjekt.name = Datenbank[4059549000152].name;

      }  

      if(result == "50000112630299"){
        this.visible=true;
        document.getElementById("ButtonID").style.background='#008000';
        this.PrintObjekt.name = Datenbank[50000112630299].name;
      }

    }
  }
}
</script>

<style >
#readerID {
  background-color: brown;
}
#ProductPictureID {
  width: 150px;
  height: 150px;
}
#ButtonId{
  background-color: white;
  color: black;
  border: 2px solid #4CAF50; /* Green */
  text-align: left;
}
#ProduktFensterID {
  background-color: blue;
  display: flex;
  flex-direction: row
}
#div2ID {
  display: flex;
  flex-direction: row
}
</style>
