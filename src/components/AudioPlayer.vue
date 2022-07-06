<template>
<div class="container">
    <audio :src="url" :times="times" @canplay="test" controls></audio>
    <div v-show="showBlip" class="blip">test</div>
</div>
</template>

<script>

	export default {
    
    name: 'AudioItem',
  
    props:{
            url: String,
            times:Array,
            timepos:Number
        },
    data(){
    return{
      index:0,
      showBlip:false,
      beats:this.times
    }
  },
    methods:{
        test(e){
            this.$emit('duration-time',e.srcElement.duration);
        }
        /*
    async test(e){
            console.log(this.times);
            console.log(Math.round(e.srcElement.currentTime * 10) / 10);
            console.log(this.times[this.index]);
            console.log((((e.srcElement.currentTime * 10)+1) / 10).toPrecision(2));
            console.log((((e.srcElement.currentTime +0.2) * 10) / 10));
            try{
            if(e.srcElement.currentTime<1){
            if(((e.srcElement.currentTime * 10) / 10)==this.times[this.index] || (((e.srcElement.currentTime * 10)+1) / 10).toPrecision(1)==this.times[this.index] || (((e.srcElement.currentTime * 10)+2) / 10).toPrecision(1)==this.times[this.index]){
                console.log("here");
                this.showBlip=true;
                await this.sleep(500);
                this.showBlip=false;
                this.index++;
            }
            else if(((e.srcElement.currentTime * 10) / 10)<this.times[this.index]){
                while(((e.srcElement.currentTime * 10) / 10)<this.times[this.index] && this.index>0){
                    this.index--;
                }
            }
            }
            else{
                if(((e.srcElement.currentTime * 10) / 10)==this.times[this.index] || (((e.srcElement.currentTime * 10)+1) / 10).toPrecision(2)==this.times[this.index] || (((e.srcElement.currentTime * 10)+2) / 10).toPrecision(2)==this.times[this.index]){
                this.showBlip=true;
                await this.sleep(500);
                this.showBlip=false;
                this.index++;
            }
            else if(((e.srcElement.currentTime * 10) / 10)<this.times[this.index]){
                while(((e.srcElement.currentTime * 10) / 10)<this.times[this.index] && this.index>0){
                    this.index--;
                }
            }
            }
            }
            
            catch(err){
                console.log(err);
            }

    
        
    },
     sleep(ms) {
          return new Promise(resolve => setTimeout(resolve, ms));
      },*/
    },
    }
</script>
<style scoped>
    .container{
        width: 30%;
    }
    .blip{
        width: 100px;
    height: 100px;
    border-radius: 50px;
    background-color: red;
    text-align: center;
    line-height: 100px;
    }
</style>