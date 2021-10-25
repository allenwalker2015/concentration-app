<template>
    <div class="tachyons grow ba b--white ma1 white br-100 flex flex-column justify-center items-center" :style="buttonStyle">
       <span class="measure lh-copy"> {{title}} </span>
       <span class="measure lh-copy subtitle"> {{subtitle}} </span>
    </div>
</template>
<script>
export default {
    props:{
        title:String,
        subtitle:String,
        size:{type:String,default:'100px'}
    },
    data(){
        return {
            buttonStyle:{
            height:this.size,
            width:this.size,
            },
        };
    }
}
</script>
<style scoped>
.subtitle{
    font-size: 8px;
}
</style>
