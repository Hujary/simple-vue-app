<template>
    <div> 
      <h1 style="text-align: center;"> Barcode App</h1>
      <StreamBarcodeReader @decode="onDecode" @loaded="onLoaded" id="readerID"></StreamBarcodeReader>
    </div>
    <div id="div2ID">
      <button id="ButtonID" disabled="disabled"> Recognized </button>
      <p id="ScannDataID" style="padding-left: 30px"> Information: {{ this.scannDataNumber }}</p>
    </div>
    <div id="ProduktFensterID" v-if="visible==true"> 
      <div > 
        <img id="ProductPictureID" :src=localDataObject.src />  <!-- Funktioniert nicht !!?-->
      </div>
      <div> 
        <ul>
          <li> {{ this.localDataObject.name }} </li> 
          <li> {{ this.localDataObject.preis + " €"}} </li> 
          <li> {{ this.localDataObject.größe + " Liter"}} </li>
        </ul>
      </div>
    </div>
</template>

<script>
import { StreamBarcodeReader } from "vue-barcode-reader";
export default {
  data() {
    return {
      scannDataNumber: "ean",
      visible: false,

      //lokales Datenobjekt für Produktausgabe  ->  bei Start "leer"
      localDataObject: {
        name: null,
        preis: null, 
        größe: null,    
        src: "https://m.media-amazon.com/images/W/IMAGERENDERING_521856-T1/images/I/61MypS1KawL._AC_SS450_.jpg",
      },

//------------------------------------------------------------------------------------------------------------------------------------

          //  "lokale" Datenbank  -->  hier wäre eigentlich eine API/Datenbank im einsatz 
       datenbank: [
        { "ean": 123456789012,
	        "name": "Cola Light",
	        "preis": 1.29,
          "größe": 1,
	        "pictureLink": "https://m.media-amazon.com/images/W/IMAGERENDERING_521856-T1/images/I/61MypS1KawL._AC_SS450_.jpg"
	      },
	       
        { "ean": 7290011018184,
	        "name": "Fuze Tee",
	        "preis": 1.19,
          "größe": 1,
	        "pictureLink": "https://www.worldofsweets.de/out/pictures/master/product/1/fuzetea-schwarzer-tee-pfirsich-400ml-no1-4837.jpg"
	      }
      ],

//------------------------------------------------------------------------------------------------------------------------------------

    }
  },
  components: {
    StreamBarcodeReader
  },
   methods: {
      //  On Decode -> sobald ein Code erkannt wurde.    (result = Nummer)
    onDecode (result) { 
      this.scannDataNumber = result; 
      
      for(let i=0; i<2; i++){                                               //   Anzahl-Elemente im Array
        if(result == this.datenbank[i].ean){                                //   prüft ob EAN in "Datenbank" liegt
          console.log("match mit:" + this.datenbank[i].ean);
          document.getElementById("ButtonID").style.background='#008000';   //   Button wird grün bei match
          this.localDataObject.name  = this.datenbank[i].name;              //   Name  übergeben
          this.localDataObject.preis = this.datenbank[i].preis;             //   Preis übergeben
          this.localDataObject.größe = this.datenbank[i].größe;             //   Größe übergeben
          this.localDataObject.src   = this.datenbank[i].src;               //   SRC   übergeben
          this.visible=true;
        } else {
          document.getElementById("ButtonID").style.background='#f82c00';   // Button wird rot bei keinem match
          //this.visible=false;
        } 
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
