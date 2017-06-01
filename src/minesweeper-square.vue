<template>
	<div class="minesweeper-square" :class="classes" :style="position" @click="reveal">
		{{ neighbours }}
	</div>
</template>
<script>
	import bus from './utils/bus'

	export default {
		name: 'minesweeper-square',
		props: ['s'],
		data() {
			return {
				playing: true
			}
		},
		computed: {
			position() {
				return {
					top: this.s.row * 30 + 'px',
					left: this.s.column * 30 + 'px',
				}
			},
			neighbours() {
				return this.s.neighbours ? this.s.neighbours : ' ';
			},
			classes() {
				return {
					'button': this.s.status
				}
			}
		},
		methods: {
			reveal() {
				if(!this.s.status && this.playing)
					bus.$emit('reveal', this.s.id);
			}
		},
		mounted() {
			bus.$on('finish', () => this.playing = false);
		}
	}
</script>
<style>
	.minesweeper-square {
		height: 30px;
		width: 30px;
		position: absolute;
		display: flex;
		align-items: center;
		justify-content: center;
		border: 1px solid #eee;
	}
	.button {
  		border: none;
  	}
</style>