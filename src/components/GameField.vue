<style scoped>
.grid-warpper {
 display: grid;
   grid-template-columns: 100px 100px 100px 100px;
   grid-gap: 1px;
}
.grid-warpper > div {
  padding: 15px;
  text-align: center;
  border: 1px solid black;
  margin:0 -1px -1px 0;
}
</style>
<template>
<div class="grid-warpper">
  <div v-for="item in rows" v-bind:key="item.index" style="height:100px" >
    <GameTile v-bind:icon="item.icon" v-bind:index="item.index" v-on:tile-clicked="tileClicked" v-bind:isOn="item.isOn" v-bind:state="item.state" ></GameTile>
  </div>
</div>
</template>
<script>
var size=4;
var fields = new Array();
var iconsTofind=["fa-address-book","fa-baby","fa-university","fa-bars","fa-biking","fa-female","fa-bug","fa-calendar","fa-bell","fa-blind"];

import GameTile from './GameTile.vue'

export default {
  name: "GameField",
  props: {
    msg: String
  },
  components:{ GameTile},
  methods:{
    tileClicked(tile){
        if(tile.state=="unknown"){
          fields[tile.index].state="try";
        }
        var tries = fields.filter(function(item){
          return item.state=="try";
        })
        if(tries.length>2){
          fields[tile.index].state="unknown";
          return;
        }
          
        if(tries.length==2 ){
          if(tries[0].icon==tries[1].icon){
              fields[tries[0].index].state="locked";
              fields[tries[1].index].state="locked";
          }
          else{
            setTimeout(function(){
              fields[tries[0].index].state="unknown";
              fields[tries[1].index].state="unknown";
            },1000);
              
          }
        }
        
          
        

        
    }
    ,
    isFull(ar){
      return ar.findIndex(function(e){
        return e.icon==null;
      })==-1
    },
    getIndex(){
      if(fields.findIndex(function(e){return e.icon==null;})==-1)
        return -1;
      for(;;){
        var idx = Math.floor(Math.random() * (size*size));
        var v = fields[idx];
        if( v.icon==null){
          return idx;
        }
      }
    }
    
  },
  data:function(){
    for(let i=0;i<size*size;i++){
      fields.push({index:i,icon:null,isOn:false,state:"unknown"});
    }
    for(;;){
      var icon = iconsTofind.pop();
      console.log(icon); 
      var firstIndex = this.getIndex();
      if(firstIndex==-1)
        break;
      fields[firstIndex].icon=icon;
      console.log(firstIndex);
      var secondIndex = this.getIndex();
      console.log(secondIndex);
      fields[secondIndex].icon=icon;
    }
    return {
      rows:fields
    };
  }
};

</script>