<template>
    <div class="sidebar" v-if="visible">
        
        <p v-for="(Item, count) in ProductData" :key="count"> 
            {{ Item.ean }} <br/>
            {{ Item.name }} <br/>
            {{ Item.price }} <br/>
            {{ Item.size }} <br/>
        </p>
    </div>
</template>

<script>
    export default {
        data(){
            return{
                visible: true,
                ProductData: [
                    {   
                        "ean": 123456789012,
	                    "name": "Cola Light",
	                    "price": 1.29,
                        "size": 1,
                    }

                ]
            }
        },
        methods: {
            async getDataFromAxios(){
                await axios.get(`https://simple-vue-app-group-ezpz.azurewebsites.net/product`)   
                .then((response) => { 
                console.log(response.data);
                this.ProductData = response.data;
                console.log(this.ProductData);
                })
            },
        },
        
    }
</script>

<style>

.sidebar{
    height: 100%;
    width: 100%;
    background-color: aqua;
}

</style>