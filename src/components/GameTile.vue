<template>
  <button @click="click" class="btn" >
      <div v-if="!tileIsOn">
        <i  class="fa fa-3x fa-question-circle" aria-hidden="true"></i>
      </div>
      <div>
        <div v-if="tileIsOn">
        <i  class="fa fa-3x" v-bind:class="iconClass" aria-hidden="true"></i>
      </div>
      </div>
       
    </button>
    
</template>

<script>
export default {
  name: "GameTile",
  props: {
    icon: String,
    index:Number,
    isOn:Boolean,
    state:String
  },
  data:function(){
    return {
      tileIsOn:false,
      iconClass:this.icon
    }
  },
  methods:{
    click:function(){
      this.$emit("tile-clicked",{index:this.index,icon:this.icon,isOn:this.tileIsOn,state:this.state})
    }
  },
  watch:{
    "isOn":function(){
      this.tileIsOn = this.isOn;
    },
    "state":function(){
      switch(this.state){
        case "unknown":
          this.tileIsOn=false;
          break;
        case "try":
          this.tileIsOn=true;
          break;
        case "locked":
          this.tileIsOn=true;
          break;
      }
    }
  }
};
</script>

