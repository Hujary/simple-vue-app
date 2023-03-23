<template>
    <div> 
      <h1> Barcode App</h1>
      <StreamBarcodeReader @decode="onDecode" @loaded="onLoaded" id="readerID"></StreamBarcodeReader>
    </div>

    <div id="div2ID">
      <button id="ButtonID" disabled="disabled"> Recognized </button>
      <p id="ScannDataID" style="padding-left: 30px"> Information: {{ scannDataNumber }}</p>
    </div>

    <div id="ProduktFensterID" v-if="visible==true"> 
      <div > 
        <img id="ProductPictureID" :src="Datenbank[this.scannDataNumber].src" /> <!-- Produktfoto -->
      </div>
      <div> 
        <ul>
          <li> {{ localDataObject.name }} </li> 
          <li> {{ localDataObject.Preis }} </li> 
          <li> {{ localDataObject.Größe }} </li>
        </ul>
      </div>
    </div>
</template>

<script>
import { StreamBarcodeReader } from "vue-barcode-reader";
export default {
  data() {
    return {
      scannDataNumber: "text",
      visible: false,

      //lokales Datenobjekt für Produktausgabe
      localDataObject: {
        name: null,
        preis: null,
        pfand: null,
        größe: null, 
        src: null,
      },

//------------------------------------------------------------------------------------------------------------------------------------

       //lokale Datenbank  --> hier wäre eigentlich eine API/Datenbank im einsatz 
      Datenbank: { 
        4059549000152: {
        name: "Atla",
        preis: 2.99,
        PfandStatus: false,
        größe: 72, 
        src: "https://m.media-amazon.com/images/W/IMAGERENDERING_521856-T1/images/I/61MypS1KawL._AC_SS450_.jpg"
        },
        123456789012: {
          name: "Cola Light", 
          Preis: 1.19,
          Pfand: 0.25,
          PfandStatus: true,
          Größe: 1, 
          src: "https://rexroth-liefert.de/media/9b/17/be/1638257642/6527397_SHOP_CCL_1L_PET_FR_GER_D_CON_EAN5449000017895_R1.png"
        },
        7085840170697: {
          name: "Fuze Tee", 
          preis: 1.29,
          pfand: 0.25,
          PfandStatus: true,
          Größe: 1, 
          src: "https://www.worldofsweets.de/out/pictures/master/product/1/fuzetea-schwarzer-tee-pfirsich-400ml-no1-4837.jpg"
        }
      }

//------------------------------------------------------------------------------------------------------------------------------------

    }
  },
  components: {
    StreamBarcodeReader
  },
   methods: {
      //On Decode -> sobald ein Code erkannt wurde    (result = Nummer)
    onDecode (result) { 
    this.scannDataNumber = result; 

      if(result == "4059549000152"){ //Kreide 
        console.log("Scan war erfolgreich: " + result);
        this.visible=true;
        document.getElementById("ButtonID").style.background='#008000'; 
        this.localDataObject.name  = Datenbank[4059549000152].name;
        this.localDataObject.preis = Datenbank[4059549000152].preis;
        this.localDataObject.größe = Datenbank[4059549000152].größe;
      }  

      if(result == "123456789012"){ //Cola Light
        console.log("Scan war erfolgreich: " + result);
        this.visible=true;
        document.getElementById("ButtonID").style.background='#008000'; 
        this.localDataObject.name  = Datenbank[123456789012].name;
        this.localDataObject.preis = Datenbank[123456789012].preis;
        this.localDataObject.größe = Datenbank[123456789012].größe;
      } 
     
      if(result == "7085840170697"){ //Fuze Tee
        console.log("Scan war erfolgreich: " + result);
        this.visible=true;
        document.getElementById("ButtonID").style.background='#008000'; 
        this.localDataObject.name  = Datenbank[7085840170697].name;
        this.localDataObject.preis = Datenbank[7085840170697].preis;
        this.localDataObject.größe = Datenbank[7085840170697].größe;
      }
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
