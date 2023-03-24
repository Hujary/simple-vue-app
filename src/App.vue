<template>
  <div id="mainWindow">
    <button class="SideBarButton" @click="toggle" > side </button>
    <div id="SideBar">
      <SideBar v-if="isSideBarActive"/>
    </div>

    <div id="Scanner" v-if="hidden">
      <div> 
        <h1 style="text-align: center;"> Barcode App </h1>
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
            <li> {{ this.localDataObject.price + " €"}} </li> 
            <li> {{ this.localDataObject.size + " Liter"}} </li>
          </ul>
        </div>
      </div>
    </div>
  </div>  
</template>

<script>
import SideBar from "./SideBar.vue";
import { StreamBarcodeReader } from "vue-barcode-reader";
import axios from 'axios';

export default {
  data() {
    return {
      scannDataNumber: "barcode",
      visible: false,
      isSideBarActive: false,
      hidden: true,
      prüfvariable: false,

        //  lokales Datenobjekt für Produktausgabe  ->  bei Start "leer"
      localDataObject: {
        name: null,
        price: null, 
        size: null,    
        src: null,
      },

//------------------------------------------------------------------------------------------------------------------------------------

          //  "lokale Datenbank"  -->  für gespeicherte Produkte -> welche erkannt werden. 
       datenbank: [
        { "ean": 123456789012,
	        "name": "Cola Light",
	        "price": 1.29,
          "size": 1,
	        "src": "https://rexroth-liefert.de/media/9b/17/be/1638257642/6527397_SHOP_CCL_1L_PET_FR_GER_D_CON_EAN5449000017895_R1.png"
	      },
	       
        { "ean": 7290011018184,
	        "name": "Fuze_Tee",
	        "price": 1.19,
          "size": 1,
	        "src": "https://www.worldofsweets.de/out/pictures/master/product/1/fuzetea-schwarzer-tee-pfirsich-400ml-no1-4837.jpg"
	      },
        { "ean": 1231231231232,
	        "name": "Fanta",
	        "price": 1.49,
          "size": 1,
	        "src": "https://cdn02.plentymarkets.com/q7p0kwea05gv/item/images/4287/full/35572c.jpg"
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
   async onDecode (result) { 

          //  variablen 
      this.visible = false;      
      this.prüfvariable = false;
      this.scannDataNumber = result;
      console.log("ean wurde erfasst");
      
      for(let i=0; i<3; i++){                                               //   Anzahl-Elemente im Array
        if(this.scannDataNumber == this.datenbank[i].ean){                  //   prüft ob EAN in "Datenbank" liegt
          console.log("match mit:" + this.datenbank[i].ean);
          document.getElementById("ButtonID").style.background='#008000';   //   Button wird grün bei match
          this.localDataObject.name  = this.datenbank[i].name;              //   Name  übergeben
          this.localDataObject.price = this.datenbank[i].price;             //   Preis übergeben
          this.localDataObject.size = this.datenbank[i].size;               //   Größe übergeben
          this.localDataObject.src = this.datenbank[i].src;                 //   SRC   übergeben
          this.visible=true;
          this.prüfvariable = true;

            //  Produkt speichern
        await axios.post(`https://simple-vue-app-group-ezpz.azurewebsites.net/product`,  {
                name: this.datenbank[i].name,
                price: this.datenbank[i].price,
                size: this.datenbank[i].size,
                ean: this.datenbank[i].ean,
                src: this.datenbank[i].src,
         })   
        .then((response) => { 
           console.log(response.data);
        })
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

    toggle() {
      this.getDataFromAxios; //Daten von API Datenbank auslesen
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

  .SideBarButton{
    height: 20px;
    width: 50px;
  }

</style>
