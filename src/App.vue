<template>
  <div id="mainWindow">
    <button class="SideBarButton" @click="toggle"> Click </button>
    <div id="SideBar">
      
      <SideBar v-if="isSideBarActive"/>
    </div>

    
  <div id="Scanner" v-if="hidden">
    <div> 
      <h1 style="text-align: center;"> Barcode App</h1>
      <StreamBarcodeReader @decode="onDecode" @loaded="onLoaded" id="readerID"></StreamBarcodeReader>
    </div>
    <div id="div2ID">
      <button id="ButtonID" disabled="disabled"> Recognized </button>
      <p id="ScannDataID" style="padding-left: 30px"> EAN: {{ this.scannDataNumber }}</p>
    </div>
    
    <div id="ProduktFensterID" v-if="visible"> 
      <div > 
        <img id="ProductPictureID" :src=this.localDataObject.src />  <!-- Funktioniert nicht !!?-->
      </div>
      <div> 
        <ul style="padding-top: 35px">
          <li> {{ this.localDataObject.name }} </li> 
          <li> {{ this.localDataObject.preis + " €"}} </li> 
          <li> {{ this.localDataObject.größe + " Liter"}} </li>
        </ul>
      </div>
    </div>
    
  </div>
  
  </div>
    
</template>

<script>
import { StreamBarcodeReader } from "vue-barcode-reader";
import SideBar from "./SideBar.vue";
export default {
  data() {
    return {
      scannDataNumber: "barcode",
      visible: false,
      isSideBarActive: false,
      hidden: true,

        //  lokales Datenobjekt für Produktausgabe  ->  bei Start "leer"
      localDataObject: {
        name: null,
        preis: null, 
        größe: null,    
        src: null,
      },

//------------------------------------------------------------------------------------------------------------------------------------

          //  "lokale" Datenbank  -->  hier wäre eigentlich eine API/Datenbank im einsatz 
       datenbank: [
        { "barcode": 123456789012,
	        "name": "Cola Light",
	        "preis": 1.29,
          "größe": 1,
	        "pictureLink": "https://m.media-amazon.com/images/W/IMAGERENDERING_521856-T1/images/I/61MypS1KawL._AC_SS450_.jpg"
	      },
	       
        { "barcode": 7290011018184,
	        "name": "Fuze Tee",
	        "preis": 1.19,
          "größe": 1,
	        "pictureLink": "https://www.worldofsweets.de/out/pictures/master/product/1/fuzetea-schwarzer-tee-pfirsich-400ml-no1-4837.jpg"
	      },
        { "barcode": 1231231231232,
	        "name": "test",
	        "preis": 1.19,
          "größe": 2,
	        "pictureLink": "https://www.worldofsweets.de/out/pictures/master/product/1/fuzetea-schwarzer-tee-pfirsich-400ml-no1-4837.jpg"
	      }
      ],

//------------------------------------------------------------------------------------------------------------------------------------

    }
  },
  components: {
    StreamBarcodeReader,
    SideBar
  },
   methods: {
      //  On Decode -> sobald ein Code erkannt wurde.    (result = Nummer)
    onDecode (result) { 
      this.scannDataNumber = result; 
      this.visible = false;
      let prüfvariable;
      
      for(let i=0; i<3; i++){                                               //   Anzahl-Elemente im Array
        console.log(i);
        if(this.scannDataNumber == this.datenbank[i].barcode){              //   prüft ob EAN in "Datenbank" liegt
          console.log("match mit:" + this.datenbank[i].barcode);
          document.getElementById("ButtonID").style.background='#008000';   //   Button wird grün bei match
          this.localDataObject.name  = this.datenbank[i].name;              //   Name  übergeben
          this.localDataObject.preis = this.datenbank[i].preis;             //   Preis übergeben
          this.localDataObject.größe = this.datenbank[i].größe;             //   Größe übergeben
          this.localDataObject.barcode   = this.datenbank[i].barcode;       //   SRC   übergeben
          this.visible=true;
          this.prüfvariable = true;
        }
      } 
      if(prüfvariable == false) {
        document.getElementById("ButtonID").style.background='#f82c00';   // Button wird rot bei keinem match 
      } 
    },

    toggle() {
      this.isSideBarActive = !this.isSideBarActive;
      this.hidden = !this.hidden
    }
  } 
}
</script>

<style >

  #mainWindow{
    display: flex;
    flex-direction: column;
  }

  #Scanner{
    
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
    flex-direction: row;
    padding-top: 20px;
  }

  .SideBarButton{
    height: 20px;
    width: 50px;
  }
</style>
