<template>
	<div>
		<div class="simon">
			<Pads v-on:start-game="startGame" v-bind:isActive1="isActive1"
			v-bind:isActive2="isActive2" 
			v-bind:isActive3="isActive3"
			v-bind:isActive4="isActive4"
			v-on:clickPad1="clickPad1"
			v-on:clickPad2="clickPad2"
			v-on:clickPad3="clickPad3"
			v-on:clickPad4="clickPad4"
			v-bind:round="round"
			/>
    	</div>
		<div id="app">
    <modal
      v-show="lose"
      v-on:close="close"
    />
  </div>
		<div class="mode">
			<span>Eazy</span><input type="radio" v-model="mode" value="eazy">
			<br/>
			<br/>
			<span>Normal</span><input type="radio" v-model="mode" value="normal">
			<br/>
			<br/>
			<span>Hard</span><input type="radio" v-model="mode" value="hard">
			<br/>
			<br/>
			<p>{{ mode }}</p>
		</div>
	</div>
</template>


<script>
import Pads from "@/components/pads"  
import modal from "@/components/modalForm"
export default {
	components: {
		Pads,
		modal
	},
	data() {
    return {
      sequence: [],
		  copy: [],
		  round: 0,
		  active: true,
		  mode: 'eazy',
		  time1: 800,
		  time2: 400,
		  isActive1: false,
		  isActive2: false,
		  isActive3: false,
		  isActive4: false,
		  lose: false,
    }
  },
	methods: {
		startGame() {
			this.isActive1 = false;
			this.isActive2 = false;
			this.isActive3 = false;
			this.isActive4 = false;
      		this.sequence = [];
			this.copy = [];
			this.round = 0;
			this.active = true;
			this.newRound();
		},
		newRound() {
      		this.sequence.push(this.randomNumber());
			this.copy = this.sequence.slice(0);
			this.animate(this.sequence);
			this.round = this.round + 1;
    },
    registerClick(e) {
			var desiredResponse = this.copy.shift();
			var actualResponse = e;
			this.active = (desiredResponse === actualResponse);
			this.checkLose();
    },
    checkLose() {
			if (this.copy.length === 0 && this.active) {
				this.newRound();
			} else if (!this.active) { 
				this.endGame();
			}
    },
    endGame() {
			this.isActive1 = false;
			this.isActive2 = false;
			this.isActive3 = false;
			this.isActive4 = false;
      		this.sequence = [];
			this.copy = [];
			var that = this;
			if(that.round > 0) {
				that.showModal();
			} 
			this.round = 0;
    },
    changeMode() {
			if(this.mode == 'eazy') {
				this.time1 = 3000;
				this.time2 = 1500;
			} else if(this.mode == 'normal') {
				this.time1 = 2000;
				this.time2 = 1000;
			} else if(this.mode == 'hard') {
				this.time1 = 800;
				this.time2 = 400;
			}
	},
	showModal() {
        this.lose = true;
      },
      close() {
        this.lose = false;
      },
	clickPad1() {
					const e = 1;
					if(this.round > 0) {
						this.playSound(e);	
					}
					this.registerClick(e);
	},
	clickPad2() {
					const e = 2;
					if(this.round > 0) {
						this.playSound(e);	
					}
					this.registerClick(e);
	},
	clickPad3() {
					const e = 3;
					if(this.round > 0) {
						this.playSound(e);	
					}
					this.registerClick(e);
	},
	clickPad4() {
					const e = 4;
					if(this.round > 0) {
						this.playSound(e);	
					}
					this.registerClick(e);
				},
		animate(sequence) {
			this.changeMode();
			var i = 0;
			var that = this;
			var interval = setInterval(function() {
				that.playSound(sequence[i]);
				that.lightUp(sequence[i]);
				
				i++;
				if (i >= sequence.length) {
					clearInterval(interval);
				}
			}, that.time1);
		},
		lightUp(tile) {
			this.changeMode();
				if (tile === 1) {
					this.isActive1 = true;
				} if (tile === 2) {
					this.isActive2 = true;
				} if (tile === 3) {
					this.isActive3 = true;
				} if (tile === 4) {
					this.isActive4 = true;
				}

				var that = this;
				setTimeout(function() {
					that.deleteAnimate();
				}, that.time2);
	},
	deleteAnimate() {
					this.isActive1 = false;
					this.isActive2 = false;
					this.isActive3 = false;
					this.isActive4 = false;
	},
    playSound(tile) {
				if(tile == 1) {
					var audio = new Audio(require('./sounds/1.mp3'));
   					audio.play();
				} else if (tile == 2) {
					var audio = new Audio(require('./sounds/2.mp3'));
   					audio.play();
				} else if (tile == 3) {
					var audio = new Audio(require('./sounds/3.mp3')); 
   					audio.play();
				} else {
					var audio = new Audio(require('./sounds/4.mp3')); 
   					audio.play();
				}
		},
		randomNumber() {
			return Math.floor((Math.random()*4)+1);
		}
		
	}
	  
}
</script>


<style scoped>
.simon {
	background: rgb(53, 47, 47);
	float: right;
	margin-right: 38em;
	width: 320px;
	height: 320px;
	border-radius: 200px 200px 200px 200px;
}
.mode {
	width: 100px;
	height: 100px;
	padding-top: 130px;
    padding-left: 220px;
    padding-bottom: 100px;
}

</style>