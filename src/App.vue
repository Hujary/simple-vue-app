<template>
    <div> 
      <h1 style="text-align: center;"> Barcode App</h1>
      <button @click=" this.getDataFromAxios()"> Sidebar </button>
      <button @click=" this.saveData()"> Objekt Speichern </button>
      <StreamBarcodeReader @decode="onDecode" @loaded="onLoaded" id="readerID"></StreamBarcodeReader>
    </div>
    <div id="div2ID">
      <button id="ButtonID" > Recognized </button>
      <p id="ScannDataID" style="padding-left: 30px"> EAN: {{ this.scannDataNumber }}</p>
    </div>
    <div id="ProduktFensterID" v-if="visible"> 
      <div > 
        <img id="ProductPictureID" :src="this.localDataObject.src" />  <!-- Funktioniert nicht !!?-->
      </div>
      <div> 
        <ul style="padding-top: 35px">
          <li> {{ this.localDataObject.name }} </li> 
          <li> {{ this.localDataObject.preis + " €"}} </li> 
          <li> {{ this.localDataObject.größe + " Liter"}} </li>
        </ul>
      </div>
    </div>
</template>

<script>

import { StreamBarcodeReader } from "vue-barcode-reader";
import axios from 'axios';

export default {
  data() {
    return {
      scannDataNumber: "barcode",
      visible: false,
      prüfvariable: false,

        //  lokales Datenobjekt für Produktausgabe  ->  bei Start "leer"
      localDataObject: {
        name: null,
        preis: null, 
        size: null,    
        src: null,
      },

//------------------------------------------------------------------------------------------------------------------------------------

          //  "lokale Datenbank"  -->  für gespeicherte Produkte -> welche erkannt werden. 
       datenbank: [
        { "ean": 123456789012,
	        "name": "Cola Light",
	        "preis": 1.29,
          "size": 1,
	        "src": "https://m.media-amazon.com/images/W/IMAGERENDERING_521856-T1/images/I/61MypS1KawL._AC_SS450_.jpg"
	      },
	       
        { "ean": 7290011018184,
	        "name": "Fuze Tee",
	        "preis": 1.19,
          "size": 1,
	        "src": "https://www.worldofsweets.de/out/pictures/master/product/1/fuzetea-schwarzer-tee-pfirsich-400ml-no1-4837.jpg"
	      },
        { "ean": 1231231231232,
	        "name": "Fanta",
	        "preis": 1.49,
          "size": 1.5,
	        "src": "https://cdn02.plentymarkets.com/q7p0kwea05gv/item/images/4287/full/35572c.jpg"
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
   async onDecode (result) { 

          //  variablen 
      this.visible = false;      
      this.prüfvariable = false;
      this.scannDataNumber = result;
      
      for(let i=0; i<3; i++){                                               //   Anzahl-Elemente im Array
        console.log(i);
        if(this.scannDataNumber == this.datenbank[i].barcode){              //   prüft ob EAN in "Datenbank" liegt

          //  Produkt speichern
        await axios.post(`https://simple-vue-app-group-ezpz.azurewebsites.net/product`,  {
                name: this.datenbank[i].name,
                price: this.datenbank[i].preis,
                size: this.datenbank[i].size,
                ean: this.datenbank[i].ean,
                src: this.datenbank[i].src,
         })   
        .then((response) => { 
          console.log("wird ausgeführt");
           console.log(response);
        }) 
      
          console.log("match mit:" + this.datenbank[i].barcode);
          document.getElementById("ButtonID").style.background='#008000';   //   Button wird grün bei match
          this.localDataObject.name  = this.datenbank[i].name;              //   Name  übergeben
          this.localDataObject.preis = this.datenbank[i].preis;             //   Preis übergeben
          this.localDataObject.size = this.datenbank[i].size;             //   Größe übergeben
          this.localDataObject.src = this.datenbank[i].src;         //   SRC   übergeben
          this.visible=true;
          this.prüfvariable = true;
        }
      } 
      if(this.prüfvariable == false) {
        document.getElementById("ButtonID").style.background='#f82c00';   // Button wird rot bei keinem match 
      } 
    },


        //   Daten von Datenbank auslesen und in response speichern
    async getDataFromAxios(){
      await axios.get(`https://simple-vue-app-group-ezpz.azurewebsites.net/product`)   
        .then((response) => { 
           console.log(response.data);
        })
    },
      //   Daten in Datenbank speichern
    async saveData() {
      await axios.post(`https://simple-vue-app-group-ezpz.azurewebsites.net/product`,  {
                name: this.datenbank[0].name,
                price: this.datenbank[0].preis,
                size: this.datenbank[0].size,
                ean: this.datenbank[0].ean,
                src: this.datenbank[0].src,
      })   
      .then((response) => { 
         console.log(response.data);
      })
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
