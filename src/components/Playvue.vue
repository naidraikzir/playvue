<template>
<div class="player">
	<audio ref="audio">
		<source :src="file" type="audio/mp3">
		Your browser does not support audio out of the box. Please update your browser.
	</audio>
	<div class="_toggle" :class="toggle" @click="play">
		<div></div>
	</div>
	<div class="_seekbar">
		<input type="range" min="0" max="1" step="0.01" :value="progress" @input="seek">
		<div class="_slider-fore" :style="{ width: `${progress*100}%` }"></div>
		<div class="_slider-back"></div>
	</div>
	<div class="_mute" @click="mute">
		<i class="_volume-full" :class="{ '-muted': muted }"></i>
	</div>
	<div class="_volume">
		<input type="range" v-model="volume" min="0" max="1" step="0.01">
		<div class="_slider-fore" :class="{ '-muted': muted }" :style="{ width: `${volume*100}%` }"></div>
		<div class="_slider-back"></div>
	</div>
</div>
</template>

<script>
export default {
	name: 'playvue',

	props: {
		file: {
			type: String,
			default: null
		}
	},

	data () {
		return {
			playing: false,
			progress: 0,
			volume: 1,
			muted: false,
		}
	},

	computed: {
		toggle () {
			return this.playing ? '-pause' : '-play'
		},
	},

	watch: {
		volume (val) {
			this.setVolume(val)
		},
		muted (val) {
			this.$player.muted = val
		}
	},

	mounted () {
		this.init()
	},

	methods: {
		init () {
			let states = [ 'pause', 'ended' ]
			this.$player = this.$refs['audio']
			this.$player.addEventListener('timeupdate', () => { this.timeUpdate() })
			for (let state of states) {
				this.$player.addEventListener(state, () => { this.playing = false })
			}
		},
		play () {
			if (this.$player.paused && this.$player.readyState == 4) {
				this.$player.play()
				this.playing = true
			}
			else this.$player.pause()
		},
		setVolume (val) {
			this.$player.volume = val
			if (this.muted) this.muted = false
		},
		seek (e) {
			this.$player.currentTime = parseFloat(e.target.valueAsNumber) * this.$player.duration
		},
		mute () {
			this.muted = !this.muted
		},
		timeUpdate () {
			this.progress = this.$player.currentTime / this.$player.duration
		}
	}
}
</script>

<style>
.player {
	display: flex;
	justify-content: space-between;
	width: 100%;
	height: 30px;
	background-color: #41b883;
	box-shadow: 0 6px 12px 0 rgba(65, 184, 131, 0.5);
	text-align: initial;
}

[type=range],
._toggle,
._mute {
	cursor: pointer;
}

._toggle,
._mute {
	flex: 0 0 36px;
}

._toggle {
	padding: 7px 11px;
}

._toggle > div {
	transition: 0.3s;
	transform-origin: right;
}

.-play > div {
	border-color: transparent transparent transparent #35495e;
	border-style: solid;
	border-width: 8px 2px 8px 12px;
}

.-pause > div{
	border-color: transparent #35495e;
	border-style: solid;
	border-width: 0 5px;
	width: 14px;
	height: 16px;
}

._seekbar {
	flex: 1.5 1 150px;
}

._volume {
	flex: 0.1 0 80px;
	margin-right: 12px;
}

._volume-full {
	border-color: transparent #35495e;
	border-style: solid;
	border-width: 5px 6px 5px 0;
	box-shadow: inset 4px 0 #35495e;
	display: inline-block;
	height: 18px;
	margin: 6px 0 0 8px;
	padding: 0px 2px;
	position: relative;
}

._volume-full.-muted::before {
	opacity: 0;
}

._volume-full::before {
	border: 6px double #35495e;
	border-top-color: transparent;
	border-bottom-color: transparent;
	border-left-color: transparent;
	border-radius: 50%;
	content: '';
	left: -4px;
	height: 12px;
	position: absolute;
	top: -8px;
	transition: 0.3s;
	width: 14px;
}

._seekbar,
._volume {
	position: relative;
}

._seekbar > input,
._volume > input {
	height: 100%;
	margin: 0;
	opacity: 0;
	position: absolute;
	width: 100%;
	z-index: 1;
}

._seekbar > div,
._volume > div {
	position: absolute;
}

._slider-fore,
._slider-back {
	margin-top: 11px;
	height: 8px;
}

._slider-fore {
	background-color: #35495e;
	transition: 0.3s opacity;
}

._slider-fore.-muted {
	opacity: 0;
}

._slider-back {
	background-color: rgba(53, 73, 94, 0.5);
	width: 100%;
}
</style>
