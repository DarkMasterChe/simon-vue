<template>
    <div class="pad" >
        <div style="margin-top: 7px">
            <div class="panel top-left-pad active" v-on:click="$emit('clickPad1')" id="1" tile="1" v-bind:class="{ isActive: isActive1 }"></div>
            <div class="panel top-right-pad active" v-on:click="$emit('clickPad2')" id="2" tile="2" v-bind:class="{ isActive: isActive2 }"></div>
        </div>
        <div>
            <div class="panel bottom-left-pad active" v-on:click="$emit('clickPad3')" id="3" tile="3" v-bind:class="{ isActive: isActive3 }"></div>
            <div class="panel bottom-right-pad active" v-on:click="$emit('clickPad4')" id="4" tile="4" v-bind:class="{ isActive: isActive4 }"></div>
        </div>
        <div style="margin-top: 15px;">
           
          <button style="margin-top: 10px" v-on:click="$emit('start-game')">START</button>
        </div>
		<span v-bind:class="{ lose: lose }"></span>
		<div style="margin-top: 10px">Round: {{ round }}</div>
    </div>
</template>

<script>
export default {
	props:['isActive1', 'isActive2', 'isActive3', 'isActive4', "round", "lose"],
  data() {
    return {
		  mode: 'eazy',
    }
  },
}
</script>

<style scoped>
    .panel {
        width: 150px;
        height: 150px;
        display: inline-block;
        cursor: pointer;
    }
    .panel:hover {
        border: black solid 1px;
    }
    .active:hover  {
        opacity: 0.6;
    }

    .top-left-pad {
        border-top-left-radius: 180px;
        background-color: red;
    }
    .top-right-pad {
        border-top-right-radius: 180px;
        background-color: blue;
    }
    .bottom-left-pad {
        border-bottom-left-radius: 180px;
        background-color: yellow;
        
    }
    .bottom-right-pad {
        border-bottom-right-radius: 180px;
        background-color: green;
        
    }
	.isActive {
		opacity: 0.6;
	}
</style>