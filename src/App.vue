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
        this.SaveObject(4059549000152);
      }  

      if(result == "50000112630299"){
        this.SaveObject(50000112630299);
      }
    }
  },

  SaveObject(produktnummer) {
    this.PrintObjekt.name = this.Datenbank[produktnummer].name; // Name von Datenbank lokal speichern
    this.PrintObjekt.src =  this.Datenbank[produktnummer].BildURL; // IMG SRC von Datenbank lokal speichern
    document.getElementById("ButtonID").style.background='#008000';
    console.log(this.PrintObjekt);
    this.visible=true;
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
