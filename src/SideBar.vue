<template>
    <div class="sidebar" v-if="visible">
        Test
    </div>
</template>

<script>
    export default {
        data(){
            return{
                visible: true,
            }
        }
    }
</script>

<style>

.sidebar{
    height: 100%;
    width: 100%;
    background-color: aqua;
}

</style>